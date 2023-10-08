cmake_minimum_required(VERSION 3.16)
project(SimpleProfiler)

set(CMAKE_CXX_STANDARD 17)

include_directories(${PROJECT_SOURCE_DIR}/include)
include_directories(${PROJECT_SOURCE_DIR}/include/linux)

add_library(SimpleProfiler SHARED simple_profiler.cpp)

target_link_libraries(SimpleProfiler pthread)