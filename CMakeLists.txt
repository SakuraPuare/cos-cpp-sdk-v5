cmake_minimum_required(VERSION 3.10)
CMAKE_policy(SET CMP0015 NEW)
project(cos-cpp-sdk)

set(CMAKE_VERBOSE_MAKEFILE ON)

option(BUILD_UNITTEST "Build unittest" ON)
option(BUILD_DEMO "Build demo" ON)
option(BUILD_SHARED_LIB "Build shared library" ON)
option(ENABLE_COVERAGE "Enable Coverage" ON)

# Set build type if not set
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Debug")
endif()

# Configure Poco build options
set(ENABLE_DATA_MYSQL FALSE)
set(ENABLE_NETSSL ON CACHE BOOL "Enable NetSSL" FORCE)
set(ENABLE_CRYPTO ON CACHE BOOL "Enable Crypto" FORCE)
set(ENABLE_JSON ON CACHE BOOL "Enable JSON" FORCE)
set(ENABLE_XML ON CACHE BOOL "Enable XML" FORCE)
set(ENABLE_FOUNDATION ON CACHE BOOL "Enable Foundation" FORCE)

find_package(OpenSSL REQUIRED)

# Add Poco as subdirectory
add_subdirectory(third_party/poco)

# Define Poco components needed
set(POCO_LIBS 
    Poco::NetSSL 
    Poco::Net 
    Poco::Crypto 
    Poco::Util 
    Poco::JSON 
    Poco::XML 
    Poco::Foundation
)

# OS Type detection
if(APPLE)
    set(OS_TYPE "APPLE")
elseif(UNIX)
    set(OS_TYPE "LINUX")
elseif(WIN32)
    set(OS_TYPE "WINDOWS")
else()
    message(FATAL_ERROR "unknown os type")
endif()

# Coverage settings
if(ENABLE_COVERAGE)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fprofile-arcs -ftest-coverage")
endif()

# Add source directory first
add_subdirectory(src)

if(BUILD_DEMO)
    message(STATUS "Build demo")
    add_subdirectory(demo)
endif()

# Configure testing
if(BUILD_UNITTEST)
    # Enable testing right after project()
    enable_testing()

    # Include GoogleTest module
    include(GoogleTest)
    message(STATUS "Build unittest")
    
    # Configure GTest
    set(BUILD_GMOCK OFF CACHE BOOL "Builds the googlemock subproject" FORCE)
    set(INSTALL_GTEST OFF CACHE BOOL "Enable installation of googletest" FORCE)
    add_subdirectory(third_party/gtest)
    
    # Add unittest directory
    add_subdirectory(unittest)
endif()

# Coverage target
if(ENABLE_COVERAGE)
    find_program(LCOV lcov REQUIRED)
    find_program(GENHTML genhtml REQUIRED)
    
    add_custom_target(coverage
        COMMAND ${LCOV} --directory . --zerocounters
        COMMAND ctest --output-on-failure
        COMMAND ${LCOV} --directory . --capture --output-file coverage.info
        COMMAND ${GENHTML} coverage.info --output-directory coverage_report
        WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
        COMMENT "Generating coverage report..."
    )
endif()