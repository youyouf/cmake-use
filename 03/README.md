# CMake编译目标
在CMake中主要生成目标有三种：
```cmake
add_executable(main main.cpp)   #生成可执行文件目标run
add_library(math SHARED add.cpp sub.cpp)   #生成共享库目标math
add_library(math STATIC add.cpp sub.cpp)   #生成静态库目标math
```
​ 使用add_executable生成可执行目标，add_library可以用来生成共享库与静态库目标(SHARED表示共享库，STATIC表示静态库)。
## 可执行文件生成
```cmake
add_executable(mian) 
target_sources(mian PRIVATE ${SRC})#指明所用的源文件
target_include_directories(mian PRIVATE ${INCLUDE}#指明头文件路径
```
也可以采用一下方式
```
add_executable(main ${SRC}) #直接说明对应的源文件
target_include_directories(main PRIVATE ${INCLUDE})
```
## 动态库与静态库
1. 方式一
```
aux_source_directory(src SRC)
message(STATUS ${SRC})
set(INCLUDE "include")
include_directories(include)  #添加搜索头文件的路径，可以多次调用

add_library(mylib_shared SHARED ${SRC})   #生成共享库目标math
add_library(mylib_static STATIC ${SRC})   #生成静态库目标math
target_include_directories(mylib_shared PUBLIC ${INCLUDE})
target_include_directories(mylib_static PUBLIC ${INCLUDE})

set_target_properties(mylib_shared PROPERTIES OUTPUT_NAME "mylib")
set_target_properties(mylib_static PROPERTIES OUTPUT_NAME "mylib")
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib) #生成的动态库保存在当前文件夹lib文件夹下

find_library(MYLIB_LIB mylib HINTS ${PROJECT_SOURCE_DIR}/lib) #查找对对应的库

add_executable(main main.cpp)
target_link_libraries(main ${MYLIB_LIB})#生成可执行文件并链接库
```
2. 方式二
```
aux_source_directory(src SRC)   #获取src目标下的所有源文件
set(INCLUDE "include")     #设置头文件路径

add_library(mylib) #默认为静态库
target_sources(my_math PRIVATE ${SRC}) #设置生成库的源文件
target_include_directories(my_math PUBLIC ${INCLUDE})  #设置库头文件


add_executable(main main.cpp)
target_link_libraries(main mylib) #生成可执行文件并链接库
```
## 常见目标属性值
1. 方式一
```cmake
#设置包含路径
target_include_directories(target_name PUBLIC include_dir)
#设置编译时预定义的宏
target_compile_definitions(target_name PUBLIC definition)
#设置编译选项
target_compile_options(target_name PUBLIC option)
#设置链接的库
target_link_libraries(target_name library_name)
#设置源文件
target_sources(target_name PUBLIC source_file)
#设置编译C++特性
target_compile_features(target_name PUBLIC feature)
```
1. 设置目标的属性还有一个通用命令

```cmake
set_target_properties(target1 target2 ...
                      PROPERTIES prop1 value1
                      prop2 value2 ...)
```

​	也就是使用`set_target_properties`指明要给哪个目标的哪个属性赋值。可以使用一行`set_target_properties`实现上述相关功能：

```cmake
set_target_properties(target_name 
                      PROPERTIES 
                      INCLUDE_DIRECTORIES include_dir
                      COMPILE_DEFINITIONS definition 
                      COMPILE_OPTIONS option
                      LINK_LIBRARIES library_name
                      SOURCES source_file
                      COMPILE_FEATURES feature)
```
3. 获取属性值
```cmake
get_target_property(<VAR> target property)
```

使用`get_target_property`可以获得目标某个属性的值，并存放到变量中供我们使用。使用下面代码可以把，我们上面赋值的属性取出来：

```cmake
get_target_property(include_dir target_name INCLUDE_DIRECTORIES)
get_target_property(definition target_name COMPILE_DEFINITIONS)
get_target_property(option target_name COMPILE_OPTIONS)
get_target_property(library_name target_name LINK_LIBRARIES)
get_target_property(source_file target_name SOURCES)
get_target_property(feature target_name COMPILE_FEATURES)
```