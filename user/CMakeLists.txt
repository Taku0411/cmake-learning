cmake_minimum_required(VERSION 3.1)
project(user)

find_package(GTest REQUIRED PATHS ../install/lib/cmake/Gtest)
find_package(foo REQUIRED PATHS ../install/cmake/)

add_executable(exe main.cpp)
target_link_libraries(exe test_lib)