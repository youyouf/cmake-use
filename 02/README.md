# 多源文件和头文件
多源文件和头文件指的是文件中目标文件引用了多个源文件及其头文件中的功能函数，此时对应的多个源文件和头文件并没有生成对应的动态库或者静态库，只是联合编译成一个可执行文件。
该方式如同如下编译：
```
g++ -o output ./src/main.cpp ./src/add.cpp ./src/sub.cpp -I ./include/
```

```
.
├── CMakeLists.txt
├── include
│   └── test.hpp
└── src
    ├── add.cpp
    ├── main.cpp
    └── sub.cpp
```
如该工程中，main.cpp函数引用了sub.cpp、add.cpp中的函数，其对应的头文件是在test.hpp中。编译器无法得知对应的情况，所以需要做一个指明。
CMakeLists.txt如下：
```cmake
cmake_minimum_required(VERSION 3.10)
project(02)

set(CMAKE_CXX_STANDARD 11)  # 将 C++ 标准设置为 C++ 11
set(CMAKE_CXX_STANDARD_REQUIRED ON)  # C++ 11 是强制要求，不会衰退至低版本
set(CMAKE_CXX_EXTENSIONS OFF)  # 禁止使用编译器特有扩展

if(NOT CMAKE_BUILD_TYPE)
	message(WARNING "NOT SET CMAKE_BUILD_TYPE")
    set(CMAKE_BUILD_TYPE "Release")
endif()

aux_source_directory(src SRC)
message(STATUS ${SRC})
set(INCLUDE "include")

add_executable(main ${SRC})
target_include_directories(main PRIVATE ${INCLUDE})
```
## 使用aux_source_directory获取所有源文件
上述CMakeLists.txt中是获取src下面的所有cpp源文件，并将记录保存到SRC中。通过message打印对应的SRC数据，可以看到如下：
```shell
-- src/add.cpp src/main.cpp src/sub.cpp
```
添加生成可执行文件main
```
add_executable(main ${SRC})
```
将上述SRC中所有的源文件生成可执行文件main
## 目标添加文件包含路径
target_include_directories是给main目标添加**包含目录**这个属性。也就是目标需要找到对应的头文件。**PRIVATE是指明作用域的**

