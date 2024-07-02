cmake_minimum_required(VERSION 3.10)
project(02)

set(CMAKE_CXX_STANDARD 11)  # 将 C++ 标准设置为 C++ 11
set(CMAKE_CXX_STANDARD_REQUIRED ON)  # C++ 11 是强制要求，不会衰退至低版本
set(CMAKE_CXX_EXTENSIONS OFF)  # 禁止使用编译器特有扩展

find_package(OpenCV)
if(OpenCV_FOUND)
	message(STATUS "OpenCV found")
    message(STATUS ${OpenCV_FOUND})
    message(STATUS ${OpenCV_INCLUDE_DIR})
    message(STATUS ${OpenCV_INCLUDES})
    message(STATUS ${OpenCV_LIBRARY})
    message(STATUS ${OpenCV_LIBRARIES})
else()
    message(WARNING "not find opencv package")
endif()   

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin) #设置可执行文件输出路径

add_executable(main main.cpp)
target_link_libraries(main ${OpenCV_LIBS})#链接OpenCV库