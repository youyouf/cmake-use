cmake_minimum_required(VERSION 3.10)
project(02)

set(CMAKE_CXX_STANDARD 11)  # 将 C++ 标准设置为 C++ 11
set(CMAKE_CXX_STANDARD_REQUIRED ON)  # C++ 11 是强制要求，不会衰退至低版本
set(CMAKE_CXX_EXTENSIONS OFF)  # 禁止使用编译器特有扩展

aux_source_directory(src SRC)
message(STATUS ${SRC})
include_directories(include)

add_library(mylib) #默认为静态库
target_sources(mylib PRIVATE ${SRC}) #设置生成库的源文件
#target_include_directories(my_math PUBLIC ${INCLUDE})  #设置库头文件


add_executable(main main.cpp)
target_link_libraries(main mylib)

set(CMAKE_INSTALL_PREFIX ${CMAKE_CURRENT_SOURCE_DIR}/install/)
install(TARGETS 
            main mylib
            RUNTIME DESTINATION bin
            LIBRARY DESTINATION lib)

 install(FILES include/test.hpp DESTINATION test/include)