
# Lab01

[高精度数（大整数）除法 - 简书](https://www.jianshu.com/p/769f8ae05b89)

## NASM

### Tutorials

[NASM Tutorial lmu](https://cs.lmu.edu/~ray/notes/nasmtutorial/)

[NASM x64 Assembly](https://www.cs.uaf.edu/2017/fall/cs301/reference/x86_64.html)


### 寄存器

![](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/03/22/d3288ae47b38ead4be94368d401c21af_202303221543341.png)


### 系统调用

[Linux System Call Table for x86 64 · Ryan A. Chapman](https://blog.rchapman.org/posts/Linux_System_Call_Table_for_x86_64/)

[用NASM开发Linux x64汇编程序：Hello World - 简书](https://www.jianshu.com/p/e7430b713186)


### x86指令集
[x86 and amd64 instruction reference](https://www.felixcloutier.com/x86/)

> ld: i386 architecture of input file `div.o' is incompatible with i386:x86-64 output

This error message typically occurs when you are trying to link object files or libraries built for a different architecture than the one you are currently targeting.

In this specific case, it seems that you are trying to link a 32-bit object file `div.o` with a 64-bit output architecture. The `i386` architecture refers to 32-bit Intel processors, while `x86-64` architecture refers to 64-bit Intel processors.

To fix this error, you will need to either recompile the `div.o` object file for the x86-64 architecture, or change the output architecture to i386. If you are using a compiler, you may need to pass the appropriate command-line option to set the output architecture to i386.

If you are unsure about the architecture of your system or the object files, you can use the `file` command to check. For example, running `file div.o` will show you the architecture of the `div.o` object file.

## GDB调试
[Get Started with our GNU Debugger Tutorial | Red Hat Developer](https://developers.redhat.com/blog/2021/04/30/the-gdb-developers-gnu-debugger-tutorial-part-1-getting-started-with-the-debugger)
[介绍 | 100个gdb小技巧](https://wizardforcel.gitbooks.io/100-gdb-tips/content/)

### gdb配置

```
/etc/gdb/gdbinit
```

### 常用命令

![](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/03/24/65f6080c772b12a39ad18e7446a97ed5_202303241016852.png)

- 调试时显示汇编源码
```gdb
layout asm
```
- 调试时显示寄存器
```gdb
layout regs
```

- 单步执行`ni`和`si`
	- 与s与n的区别在于：s与n是C语言级别的单步调试，si与ni是汇编级别的单步调试


- x 打印 `.data`段数据时要加`&` (取地址)
```gdb
x /4b &did_len
```

#### print高级用法

[GDB print命令高级用法](http://c.biancheng.net/view/8252.html)

## BOCHS
### 安装
依赖库
[调试操作系统：bochs 使用入门 - 知乎](https://zhuanlan.zhihu.com/p/84984590)

### 调试
[bochs调试方法与指令详解\_guozuofeng的博客-CSDN博客](https://blog.csdn.net/guozuofeng/article/details/102646618)


[Bochs 内存查看 - 字节流](http://www.bytekits.com/bochs/bochs-show-mem-cmd.html#:~:text=Bochs%20%E5%86%85%E5%AD%98%E6%9F%A5%E7%9C%8B%201%20x%20%2Fnuf%20%5Baddr%5D%20%E6%98%BE%E7%A4%BA%E7%BA%BF%E6%80%A7%E5%9C%B0%E5%9D%80%EF%BC%88Linear,Address%EF%BC%89%E7%9A%84%E5%86%85%E5%AE%B9%202%20xp%20%2Fnuf%20%5Baddr%5D%20%E6%98%BE%E7%A4%BA%E7%89%A9%E7%90%86%E5%9C%B0%E5%9D%80%EF%BC%88Physical%20Address%EF%BC%89%E7%9A%84%E5%86%85%E5%AE%B9)




# Lab02

## extern  asm in C++

To use assembly code written in NASM with a C++ program, you can use the `extern` keyword along with the `extern "C"` syntax to declare the assembly function in your C++ code.

Here's an example of how to declare an assembly function in a C++ program:

```cpp
// assembly code (add.asm)
section .text
global add_asm
add_asm:
    mov eax, [ebp+8] ; Get first argument
    add eax, [ebp+12] ; Add second argument
    ret ; Return result

// C++ code
extern "C" {
    int add_asm(int, int);  // Declare add_asm() function as external
}

int main() {
    int a = 2, b = 3;
    int sum = add_asm(a, b);  // Call the assembly function
    return 0;
}

```

In this example, we define an assembly function `add_asm` in a separate NASM file `add.asm`. We then declare this function as `extern` in our C++ code using the `extern "C"` syntax to ensure that the function uses C linkage. Finally, we call the `add_asm` function in our `main` function.

Note that you need to make sure the assembly code is compiled into an object file that can be linked with the C++ code. You can compile the NASM code using the `nasm` command and then link the object files together using the `g++` command.

## FAT

[FAT - OSDev Wiki](https://wiki.osdev.org/FAT#FAT_12)

[檔案配置表 - 维基百科，自由的百科全书](https://zh.wikipedia.org/wiki/%E6%AA%94%E6%A1%88%E9%85%8D%E7%BD%AE%E8%A1%A8)

[File Allocation Table - Wikipedia](https://en.wikipedia.org/wiki/File_Allocation_Table#FAT12)

[https://www.eit.lth.se/fileadmin/eit/courses/eitn50/Literature/fat12\_description.pdf](https://www.eit.lth.se/fileadmin/eit/courses/eitn50/Literature/fat12_description.pdf)

## Esacpe Code

[ANSI转义序列详解 - 掘金](https://juejin.cn/post/7086720921158811662)

[Linux技巧：在代码中设置终端字符显示颜色和移动光标位置 - 南木阁 - SegmentFault 思否](https://segmentfault.com/a/1190000023553724)

```
\033[<color>m
```

## cstdint

`stdint.h` is a standard header file in the C and C++ programming languages that defines a set of typedefs to provide standardized integer types with specific widths across different platforms. These integer types include `int8_t`, `int16_t`, `int32_t`, and `int64_t` for signed integers, and `uint8_t`, `uint16_t`, `uint32_t`, and `uint64_t` for unsigned integers.

The width of these integer types is specified in bits, which makes them especially useful for low-level programming where the size of the data types is critical. By using these standard integer types, you can write code that is more portable and less likely to cause issues when compiled on different platforms with different integer sizes.

`stdint.h` is part of the C99 and C++11 standards and is widely supported by modern C and C++ compilers, including GCC, Clang, and Visual C++. It is also possible to use `stdint.h` in C++ code by including the header file `<cstdint>` instead, which provides the same functionality using C++-style `namespace` and `template` constructs.

## 读取二进制文件

[C++二进制文件的读取和写入（精华版）](http://c.biancheng.net/view/302.html)

[Binary Files in C++](https://www.eecs.umich.edu/courses/eecs380/HANDOUTS/cppBinaryFileIO-2.html)

## 查看二进制文件内容

### 命令行
- `hexdump`:display file contents in hexadecimal, decimal, octal, or ascii


```shell
An ASCII, decimal, hexadecimal, octal dump.More information: https://manned.org/hexdump.

 - Print the hexadecimal representation of a file, replacing duplicate lines by '*':
   hexdump {{path/to/file}}

 - Display the input offset in hexadecimal and its ASCII representation in two columns:
   hexdump -C {{path/to/file}}

 - Display the hexadecimal representation of a file, but interpret only n bytes of the input:
   hexdump -C -n{{number_of_bytes}} {{path/to/file}}

 - Don't replace duplicate lines with '*':
   hexdump --no-squeezing {{path/to/file}}

```

### vim
```shell
vim -b <file>
```

- 将内容转为16进制
```shell
:%!xxd
```
- 改回去，否则二进制文件可能无法使用;
	- 强制退出不保存也可以
```shell
:%!xxd -r
```


## 移位运算符优先级问题

`<<` `>>`的优先级比`+`等低

[C++ 运算符优先级 - C++中文 - API参考文档](https://www.apiref.com/cpp-zh/cpp/language/operator_precedence.html)

```cpp
#include <iostream>

  

using namespace std;

  

int main(){

int a = 1 << 2 + 3;

int b = (1 << 2) + 3;

cout << "a: " << a << endl;

cout << "b: " << b << endl;

}
```
输出：
```shell
a: 32

b: 7
```


## vscode code format

-   Shift + Alt +F in Windows
    
-   Shift + Option +F in Mac
    
-   Ctrl + Shift + I in Linux

[Site Unreachable](https://www.w3schools.io/editor/vscode-format-code/)


## core dumped处理

[Linux之core dumped出错原因及位置分析\_宗而研之的博客-CSDN博客](https://blog.csdn.net/zong596568821xp/article/details/104436395)