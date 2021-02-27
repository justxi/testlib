cmake_minimum_required(VERSION 3.13)
project(test)

set(CMAKE_BUILD_TYPE Release)

include(GNUInstallDirs)

set(LIB_VERSION_MAJOR 0)
set(LIB_VERSION_MINOR 1)
set(LIB_VERSION_TWEAK 2)
set(LIB_VERSION ${LIB_VERSION_MAJOR}.${LIB_VERSION_MINOR}.${LIB_VERSION_TWEAK})

add_subdirectory(lib)
add_subdirectory(demo)
