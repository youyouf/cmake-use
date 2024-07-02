# C、C++混合编译
&emsp;&emsp;项目工程中，常常多人分工协作，因此C、C++通常会混合编译
## 编译过程
不管是C还是C++，这些易于人类阅读的高级语言，终究要进行编译，最终转换成机器可识别的二进制语言。

编译一般分为四个步骤：预处理->编译->汇编->链接。

（一）预处理

预处理由预处理器（Preprocessor）处理，删除注释，引入头文件或者包，将宏定义内容在源文件（*.c、*.cpp等）中进行替换。

（二）编译
汇编阶段，编译器（Compiler）将经过预处理的高级语言翻译成机器可识别的汇编语言。

（三）汇编

由汇编器（Assembler），将汇编语言转换成机器语言（二进制文件），并进行重定位（Relocatable）。再由加载器（Loader）将重定向后的指令和数据放到内存指定位置。

（四）链接

最终，由链接器（Linker）将多个可重定位的机器代码文件（包括库文件）连接到一起形成可执行文件。

C++中引用C语言函数通常需要采用extern "C"做媒介，用于告诉编译器，该函数是由C语言编写。

```c
#ifdef __cplusplus
extern "C"
{
#endif
    void printHello(void);
#ifdef __cplusplus
}
#endif
```
cmake中和C++工程中的相关配置一样
```cmake
cmake_minimum_required(VERSION 3.10)
project(06)

set(CMAKE_CXX_STANDARD 11)  # 将 C++ 标准设置为 C++ 11
set(CMAKE_CXX_STANDARD_REQUIRED ON)  # C++ 11 是强制要求，不会衰退至低版本
set(CMAKE_CXX_EXTENSIONS OFF)  # 禁止使用编译器特有扩展

aux_source_directory(src SRC) #获取src文件下的源文件
include_directories(include)  #添加头文件

add_executable(main ${SRC})  #编译成可执行文件
```
## 参考链接：
https://zhuanlan.zhihu.com/p/609014977