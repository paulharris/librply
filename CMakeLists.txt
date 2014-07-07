# Minimal version of CMake
cmake_minimum_required (VERSION 2.6)

# Build type
if (NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
	message(STATUS "Setting build type to 'Release' as none was specified.")
	set(CMAKE_BUILD_TYPE Release CACHE STRING "Choose the type of build." FORCE)
	# Set the possible values of build type for cmake-gui
	set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release")
endif ()

# Define project name
project (librply)

# The version number
set (${PROJECT_NAME}_VERSION_MAJOR 1)
set (${PROJECT_NAME}_VERSION_MINOR 1)
set (${PROJECT_NAME}_PATCH_LEVEL 3)

set (${PROJECT_NAME}_VERSION
	"${${PROJECT_NAME}_VERSION_MAJOR}.${${PROJECT_NAME}_VERSION_MINOR}")

option(LIBRPLY_CLANG "Use Clang Compiler" OFF)

# Clang compiler
if (LIBRPLY_CLANG)
	set (CMAKE_C_COMPILER "/usr/bin/clang")
	set (CMAKE_CC_COMPILER "/usr/bin/clang++")
endif (LIBRPLY_CLANG)

# Set output directory for libraries
set (LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib/)

# Set output directory for binaries
set (EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin/)

# Set up required subdirectory for library
add_subdirectory (src/lib)

# Set up required subdirectory for tools
add_subdirectory (src/tools)
