cmake_minimum_required(VERSION 3.10)
project(ProjectEcho)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Set output directories
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

# Find packages
find_package(SDL2 REQUIRED)
find_package(OpenGL REQUIRED)
find_package(GLEW REQUIRED)

# Include directories
include_directories(${SDL2_INCLUDE_DIRS})
include_directories(${OPENGL_INCLUDE_DIR})
include_directories(${CMAKE_SOURCE_DIR}/src)
include_directories(${CMAKE_SOURCE_DIR}/dependencies/glm)

# Source files
file(GLOB_RECURSE SOURCES "src/*.cpp")

# Create executable
add_executable(ProjectEcho ${SOURCES})

# Link libraries
target_link_libraries(ProjectEcho 
    ${SDL2_LIBRARIES}
    ${OPENGL_LIBRARIES}
    GLEW::GLEW
)

# Platform-specific settings
if(WIN32)
    target_link_libraries(ProjectEcho opengl32)
endif()

if(APPLE)
    target_link_libraries(ProjectEcho "-framework OpenGL")
endif()

# Compiler warnings
if(MSVC)
    target_compile_options(ProjectEcho PRIVATE /W4)
else()
    target_compile_options(ProjectEcho PRIVATE -Wall -Wextra -Wpedantic)
endif()
