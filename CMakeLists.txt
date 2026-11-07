cmake_minimum_required(VERSION 3.15)

project(xsh LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(xsh src/main.cpp)

target_include_directories(xsh PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/include)