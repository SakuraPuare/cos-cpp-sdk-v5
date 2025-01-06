project(cos-cpp-sdk-ut)

# Collect source files
file(GLOB common_src src/test_utils.cpp)
file(GLOB object_request_test_src src/object_request_test.cpp)
file(GLOB object_response_test_src src/object_response_test.cpp)
file(GLOB bucket_response_test_src src/bucket_response_test.cpp)
file(GLOB util_test_src src/util_test.cpp)
file(GLOB object_op_test_src src/object_op_test.cpp)
file(GLOB bucket_op_test_src src/bucket_op_test.cpp)
file(GLOB live_channel_test_src src/live_channel_test.cpp)
file(GLOB async_op_test_src src/async_op_test.cpp)
file(GLOB auditing_req_test_src src/auditing_req_test.cpp)

# Set output directories
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

# Make sure the directories exist
file(MAKE_DIRECTORY ${CMAKE_RUNTIME_OUTPUT_DIRECTORY})
file(MAKE_DIRECTORY ${CMAKE_LIBRARY_OUTPUT_DIRECTORY})

# Function to add test targets
function(add_test_target target_name source)
    add_executable(${target_name} ${source} ${common_src})
    
    target_include_directories(${target_name} PRIVATE 
        ${CMAKE_SOURCE_DIR}/include
        ${CMAKE_CURRENT_SOURCE_DIR}/include
    )
    
    target_link_libraries(${target_name} PRIVATE 
        cossdk
        ${POCO_LIBS}
        ${SYSTEM_LIBS}
        GTest::gtest 
        GTest::gtest_main
    )
    
    add_test(
        NAME ${target_name}
        COMMAND ${target_name}
        WORKING_DIRECTORY ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}
    )
endfunction()

# Add test targets
add_test_target(object-request-test ${object_request_test_src})
add_test_target(object-response-test ${object_response_test_src})
add_test_target(bucket-response-test ${bucket_response_test_src})
add_test_target(util-test ${util_test_src})
add_test_target(object-op-test ${object_op_test_src})
add_test_target(bucket-op-test ${bucket_op_test_src})
add_test_target(live-channel-test ${live_channel_test_src})
add_test_target(async-op-test ${async_op_test_src})
add_test_target(auditing-req-test ${auditing_req_test_src})

# All-in-one test executable
add_executable(all-test
    ${object_request_test_src}
    ${object_response_test_src}
    ${bucket_response_test_src}
    ${util_test_src}
    ${object_op_test_src}
    ${bucket_op_test_src}
    ${live_channel_test_src}
    ${async_op_test_src}
    ${auditing_req_test_src}
    ${common_src}
)

target_include_directories(all-test PRIVATE 
    ${CMAKE_SOURCE_DIR}/include
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

target_link_libraries(all-test PRIVATE 
    cossdk
    ${POCO_LIBS}
    ${SYSTEM_LIBS}
    GTest::gtest 
    GTest::gtest_main
)

add_test(
    NAME all-test
    COMMAND all-test
    WORKING_DIRECTORY ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}
)

# Copy config file
file(COPY conf/config.json DESTINATION ${CMAKE_RUNTIME_OUTPUT_DIRECTORY})