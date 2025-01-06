project(cos-demo)

if (NOT ${OS_TYPE} STREQUAL "WINDOWS")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-unused-function")
endif()

# Collect source files
file(GLOB cos_demo_src "${CMAKE_SOURCE_DIR}/demo/cos_demo.cpp")
file(GLOB put_object_demo_src "${CMAKE_SOURCE_DIR}/demo/object_op_demo/put_object_demo.cpp")
file(GLOB get_object_url_demo_src "${CMAKE_SOURCE_DIR}/demo/object_op_demo/get_object_url_demo.cpp")
file(GLOB head_object_demo_src "${CMAKE_SOURCE_DIR}/demo/object_op_demo/head_object_demo.cpp")
file(GLOB get_object_demo_src "${CMAKE_SOURCE_DIR}/demo/object_op_demo/get_object_demo.cpp")
file(GLOB delete_object_demo_src "${CMAKE_SOURCE_DIR}/demo/object_op_demo/delete_object_demo.cpp")
file(GLOB put_get_object_acl_demo_src "${CMAKE_SOURCE_DIR}/demo/object_op_demo/put_get_object_acl_demo.cpp")
file(GLOB put_get_delete_object_tagging_demo_src "${CMAKE_SOURCE_DIR}/demo/object_op_demo/put_get_delete_object_tagging_demo.cpp")
file(GLOB restore_object_demo_src "${CMAKE_SOURCE_DIR}/demo/object_op_demo/restore_object_demo.cpp")
file(GLOB copy_move_object_demo_src "${CMAKE_SOURCE_DIR}/demo/object_op_demo/copy_move_object_demo.cpp")
file(GLOB multi_put_object_demo_src "${CMAKE_SOURCE_DIR}/demo/object_op_demo/multi_put_object_demo.cpp")
file(GLOB multi_get_object_demo_src "${CMAKE_SOURCE_DIR}/demo/object_op_demo/multi_get_object_demo.cpp")
file(GLOB select_objec_demo_src "${CMAKE_SOURCE_DIR}/demo/object_op_demo/select_objec_demo.cpp")
file(GLOB get_bucket_list_demo_src "${CMAKE_SOURCE_DIR}/demo/bucket_op_demo/get_bucket_list_demo.cpp")
file(GLOB delete_bucket_demo_src "${CMAKE_SOURCE_DIR}/demo/bucket_op_demo/delete_bucket_demo.cpp")
file(GLOB head_bucket_demo_src "${CMAKE_SOURCE_DIR}/demo/bucket_op_demo/head_bucket_demo.cpp")
file(GLOB put_bucket_demo_src "${CMAKE_SOURCE_DIR}/demo/bucket_op_demo/put_bucket_demo.cpp")

set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR}/bin)

# Remove unnecessary link_directories and include_directories
function(add_demo_target target_name source)
    add_executable(${target_name} ${source})
    target_include_directories(${target_name} PRIVATE 
        ${CMAKE_SOURCE_DIR}/include
    )
    target_link_libraries(${target_name} PRIVATE 
        cossdk
        OpenSSL::SSL 
        OpenSSL::Crypto
        ${POCO_LIBS}
        ${SYSTEM_LIBS}
    )
endfunction()

# Add all demo targets using the function
add_demo_target(cos_demo ${cos_demo_src})
add_demo_target(put_object_demo ${put_object_demo_src})
add_demo_target(get_object_url_demo ${get_object_url_demo_src})
add_demo_target(head_object_demo ${head_object_demo_src})
add_demo_target(get_object_demo ${get_object_demo_src})
add_demo_target(delete_object_demo ${delete_object_demo_src})
add_demo_target(put_get_object_acl_demo ${put_get_object_acl_demo_src})
add_demo_target(put_get_delete_object_tagging_demo ${put_get_delete_object_tagging_demo_src})
add_demo_target(restore_object_demo ${restore_object_demo_src})
add_demo_target(copy_move_object_demo ${copy_move_object_demo_src})
add_demo_target(multi_put_object_demo ${multi_put_object_demo_src})
add_demo_target(multi_get_object_demo ${multi_get_object_demo_src})
add_demo_target(select_objec_demo ${select_objec_demo_src})
add_demo_target(get_bucket_list_demo ${get_bucket_list_demo_src})
add_demo_target(delete_bucket_demo ${delete_bucket_demo_src})
add_demo_target(head_bucket_demo ${head_bucket_demo_src})
add_demo_target(put_bucket_demo ${put_bucket_demo_src})