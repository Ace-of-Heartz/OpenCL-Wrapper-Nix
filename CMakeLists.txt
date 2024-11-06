cmake_minimum_required(VERSION 3.29)
project(OpenCL-Wrapper LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)

find_package(OpenCL REQUIRED)

file(GLOB_RECURSE SOURCE_FILES
        ${CMAKE_SOURCE_DIR}/src/*.c
        ${CMAKE_SOURCE_DIR}/src/*.cpp)

# Add header files
file(GLOB_RECURSE HEADER_FILES
        ${CMAKE_SOURCE_DIR}/src/*.h
        ${CMAKE_SOURCE_DIR}/src/*.hpp)

add_executable(${PROJECT_NAME} ${HEADER_FILES} ${SOURCE_FILES})

target_link_libraries(${PROJECT_NAME} PRIVATE OpenCL::OpenCL)

add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_directory
        ${CMAKE_SOURCE_DIR}/kernels
        ${CMAKE_BINARY_DIR}/kernels
        COMMENT "Copy assets directory to result directory")

set_target_properties(${PROJECT_NAME} PROPERTIES
        C_STANDARD 20
        C_STANDARD_REQUIRED ON
        C_EXTENSIONS OFF)

target_compile_definitions(${PROJECT_NAME} PRIVATE CL_TARGET_OPENCL_VERSION=210)