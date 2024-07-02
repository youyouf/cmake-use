cmake_minimum_required(VERSION 3.10)
project(06)

set(CMAKE_CXX_STANDARD 11)  # 将 C++ 标准设置为 C++ 11
set(CMAKE_CXX_STANDARD_REQUIRED ON)  # C++ 11 是强制要求，不会衰退至低版本
set(CMAKE_CXX_EXTENSIONS OFF)  # 禁止使用编译器特有扩展

aux_source_directory(src SRC)
message(STATUS ${SRC})
include_directories(include)

add_executable(main ${SRC})