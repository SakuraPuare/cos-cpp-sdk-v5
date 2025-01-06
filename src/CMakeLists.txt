project(cossdk)

file(GLOB sdk_common_header "${CMAKE_SOURCE_DIR}/include/*.h")
file(GLOB sdk_op_header "${CMAKE_SOURCE_DIR}/include/op/*.h")
file(GLOB sdk_request_header "${CMAKE_SOURCE_DIR}/include/request/*.h")
file(GLOB sdk_response_header "${CMAKE_SOURCE_DIR}/include/response/*.h")
file(GLOB sdk_threadpool_header "${CMAKE_SOURCE_DIR}/include/threadpool/*.h")
file(GLOB sdk_trsf_header "${CMAKE_SOURCE_DIR}/include/trsf/*.h")
file(GLOB sdk_util_header "${CMAKE_SOURCE_DIR}/include/util/*.h")
file(GLOB sdk_header
        ${sdk_common_header}
        ${sdk_op_header}
        ${sdk_request_header}
        ${sdk_response_header}
        ${sdk_threadpool_header}
        ${sdk_trsf_header}
        ${sdk_util_header})

file(GLOB sdk_common_src "${CMAKE_SOURCE_DIR}/src/*.cpp")
file(GLOB sdk_op_src "${CMAKE_SOURCE_DIR}/src/op/*.cpp")
file(GLOB sdk_request_src "${CMAKE_SOURCE_DIR}/src/request/*.cpp")
file(GLOB sdk_response_src "${CMAKE_SOURCE_DIR}/src/response/*.cpp")
file(GLOB sdk_trsf_src "${CMAKE_SOURCE_DIR}/src/trsf/*.cpp")
file(GLOB sdk_util_src "${CMAKE_SOURCE_DIR}/src/util/*.cpp")
file(GLOB sdk_src
        ${sdk_common_src}
        ${sdk_op_src}
        ${sdk_request_src}
        ${sdk_response_src}
        ${sdk_trsf_src}
        ${sdk_util_src})

message("Sdk header: ${sdk_header}")
message("Sdk src: ${sdk_src}")

set(LIBRARY_OUTPUT_PATH ${CMAKE_BINARY_DIR}/lib)

add_library(${PROJECT_NAME} STATIC ${sdk_src} ${sdk_header})
target_include_directories(${PROJECT_NAME} PUBLIC
        ${CMAKE_SOURCE_DIR}/include
)
target_link_libraries(${PROJECT_NAME} PRIVATE ${POCO_LIBS})
set_target_properties(${PROJECT_NAME} PROPERTIES OUTPUT_NAME "cossdk")

if (BUILD_SHARED_LIB)
    message(STATUS "Build shared lib")
    add_library(${PROJECT_NAME}-shared SHARED ${sdk_src})
    target_include_directories(${PROJECT_NAME}-shared PUBLIC
            ${CMAKE_SOURCE_DIR}/include
    )
    target_link_libraries(${PROJECT_NAME}-shared PRIVATE
            ${POCO_LIBS}
            ${OPENSSL_LIBS}
            ${SYSTEM_LIBS}
    )
endif ()