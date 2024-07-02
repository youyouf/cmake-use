# 使用与查找第三方库
1. `find_pack`
&emsp;&emsp;在编写代码时，通常会使用到一些第三方库，如opencv等，我们可以直接使用find_package 来快速帮我们查找到该地三方库
```cmake
find_package(OpenCV)
```
find_package在不添加其他参数下默认就是Module模式，在Module模式下，其实是在找FindOpenCV.cmake文件，查找路径是/usr/share/cmake-xxx/Modules。Find.cmake文件是CMake官方给我们提供的，它只支持一些特别常见的库。
&emsp;&emsp;此外，我们可以判断是否找到该库，如
```cmake
find_package(OpenCV)

if(OpenCV_FOUND)
	message(STATUS "OpenCV found")
else()
	message(WARNING "OpenCV not  found")
endif()
```
另外可以使用对应的一些变量打印相关库的信息，如
```
find_package(OpenCV)

if(OpenCV_FOUND)
	message(STATUS "OpenCV found")
    message(STATUS ${OpenCV_FOUND})
    message(STATUS ${OpenCV_INCLUDE_DIR})
    message(STATUS ${OpenCV_INCLUDES})
    message(STATUS ${OpenCV_LIBRARY})
    message(STATUS ${OpenCV_LIBRARIES})
else()
	message(WARNING "OpenCV not  found")
endif(）
```
2. 非标准包查找
&emsp;&emsp;如果相关库不在cmake中存在，而是第三方编译出来的库，可以通过添加编译路径，再利用`find_pack`来进行查找[可查看链接](https://github.com/youyouf/ncnn-use/blob/main/CMakeLists.txt)
```
set(CMAKE_PREFIX_PATH ${CMAKE_PREFIX_PATH} "./ncnn")
find_package(ncnn)
```
3. 自构建库（动态库、静态库）
自构建生成的库通常有两种，一种和目标可执行文件一起构建使用的库，此时可以直接链接到该库上，另一种就是生成该动态库或者静态库后，再在其他工程中使用，此时需要使用`find_library`来进行链接