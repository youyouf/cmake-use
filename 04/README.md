# 库与可执行文件安装
当编译完成后，通常需要将生成的库与安装目录、头文件等安装到系统目录下。常常使用的install进行安装
## install安装目标
```cmake
install(TARGETS targets... [EXPORT <export-name>]
        [RUNTIME_DEPENDENCIES args...|RUNTIME_DEPENDENCY_SET <set-name>]
        [[ARCHIVE|LIBRARY|RUNTIME|OBJECTS|FRAMEWORK|BUNDLE|
          PRIVATE_HEADER|PUBLIC_HEADER|RESOURCE|FILE_SET <set-name>|CXX_MODULES_BMI]
         [DESTINATION <dir>]
         [PERMISSIONS permissions...]
         [CONFIGURATIONS [Debug|Release|...]]
         [COMPONENT <component>]
         [NAMELINK_COMPONENT <component>]
         [OPTIONAL] [EXCLUDE_FROM_ALL]
         [NAMELINK_ONLY|NAMELINK_SKIP]
        ] [...]
        [INCLUDES DESTINATION [<dir> ...]]
        )
```
使用例子
```cmake
install(TARGETS 
            target mylib
            RUNTIME DESTINATION bin
            LIBRARY DESTINATION lib)
```
安装target和mylib两个目标，RUNTIME表示可执行目标文件会被安装到bin目录下，LIBRARY表示静态库会被安装在lib下。这里的目标都是相对路径，相对于`CMAKE_INSTALL_PREFIX`这个宏，也就是实际安装在`${CMAKE_INSTALL_PREFIX}/bin`和`${CMAKE_INSTALL_PREFIX}/lib`下
使用install安装头文件
```cmake
install(<FILES|PROGRAMS> files...
        TYPE <type> | DESTINATION <dir>
        [PERMISSIONS permissions...]
        [CONFIGURATIONS [Debug|Release|...]]
        [COMPONENT <component>]
        [RENAME <name>] [OPTIONAL] [EXCLUDE_FROM_ALL])
```
例子
```cmake
 install(FILES 
                include/test.hpp DESTINATION test/include)
```

也就是将`test.hpp`安装到`${CMAKE_INSTALL_PREFIX}/test/include`下去

完整的项目工程
```
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

install(FILES include DESTINATION include)
```
编译完成后，执行`sudo make install`即可以安装到当前项目中。安装路径如下：
```shell
├── build
├── CMakeLists.txt
├── include
│   └── my_math.hpp
├── install
│   ├── bin
│   │   └── main
│   ├── lib
│   │   └── libmylib.a
│   └── test
│       └── include
│           └── my_math.hpp
├── src
│   ├── add.cpp
│   └── sub.cpp
└── main.cpp
```
