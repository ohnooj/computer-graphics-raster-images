cmake_minimum_required(VERSION 3.20)
project(raster)

# C++ standard set for C++20
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Append extra CMake modules to the module path found in cmake/
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

# Toggles student vs solution sources
option(USE_SOLUTION "Build the reference solution" OFF)
set(SRC_DIR "${CMAKE_CURRENT_SOURCE_DIR}/src")
if (USE_SOLUTION)
  set(SRC_DIR "${CMAKE_CURRENT_SOURCE_DIR}/solution")
endif()

# Find source files all under SRC_DIR with .cpp extension.
# CONFIGURE_DEPENDS re-globs if files have changed.
file(GLOB SRCFILES CONFIGURE_DEPENDS "${SRC_DIR}/*.cpp")

# Add an executable to the project using the specified source files with main.cpp
add_executable(${PROJECT_NAME} "${CMAKE_CURRENT_SOURCE_DIR}/main.cpp" ${SRCFILES})

# Specifies include directories to use when compiling a given target.
# Includes are used by only this target and not by targets that link to it.
target_include_directories(${PROJECT_NAME}
  PRIVATE
    "${CMAKE_CURRENT_SOURCE_DIR}/include"
    "${CMAKE_CURRENT_SOURCE_DIR}/json"
)

# Output Warnings
if (MSVC)
  target_compile_options(${PROJECT_NAME} PRIVATE /W4 /permissive-)
else()
  target_compile_options(${PROJECT_NAME} PRIVATE -Wall -Wextra -Wpedantic)
endif()

