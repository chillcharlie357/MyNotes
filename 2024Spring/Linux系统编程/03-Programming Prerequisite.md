---
aliases: 
tags: 
categories:
sticky:
thumbnail:
cover: 
excerpt: false
mathjax: true
comment: true
title: 03-Programming Prerequisite
date:  2024-03-25 10:03
modified:  2024-04-08 11:04
---

# 1. Programming Language

- High-level Language
	- C/C++, Java, Fortran…
	- ELF binary format
	- binary code: 本地二进制码，直接对应机器指令
- Script 
	- Shell: sh/bash, csh, ksh
	- Perl, Python, tcl/tk, sed, awk

# 2. 编译

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2024%2F03%2F25%2F11-15-22-efb7064e12a9cbfd0fdedef49bcc392a-20240325111521-19928c.png)

- 预处理
	- 把预处理指令处理掉（`#include`, `#if`,`#define`...）
	- `#include`把目标文件粘贴到include的位置
	- `#define`字符串替换

- 链接：保证可执行文件里的二进制码是完整的
	- 编译器每次只编译一个文件，链接实现了把这些单独的文件合成一个文件

- 静态库
	- 在编译时就把库文件放到可执行文件里
	- `.a` in Linux ,`.lib` in Windows
- 动态库
	- 运行时去找库文件，用完可以释放掉减少内存占用，有利于打补丁
	- `.so` in Linux, `.dll` in Windows

# 3. 其它语言

- Java
	- 只有编译+解释执行
	- 没有链接的选项
	- 只有动态库
- .Net平台：VC++.net, c#, VB.net
	- 编译成中间语言
- VC++及Delphi等（一般称为Win编程）
	- 编译成本地二进制码

# 4. GCC

## 4.1. 选项

gcc的参数和llvm差不多

- 只编译: `gcc -c test.c -o test.obj`， 输出`.obj`
- 只链接`gcc test.obj -o test`，输出可执行文件
- 编译链接一起做：`gcc test.c -o test`，

- Usage:
	- gcc [options] [filename]
- Basic options:
	1. -E: 只对源程序进行预处理(调用cpp预处理器)
	2. -S: 只对源程序进行预处理、编译
	3. -c: 执行预处理、编译、汇编而不链接
	4. -o output_file: 指定输出文件名
	5. -g: 产生调试工具必需的符号信息
	6. -O/On: 在程序编译、链接过程中进行优化处理
		1. 如果开优化就无法调试，因为优化后代码与源代码无法直接对应
		2. 在一台机器上编译，无法直接复制到另一台机器上调试
	7. -Wall: 显示所有的警告信息
	8. -Idir: 指定额外的头文件搜索路径
	9. -Ldir: 指定额外的库文件搜索路径
	10. -Iname: 链接时搜索指定的库文件
	11. -DMACRO=\[=DEFN]: 定义MACRO宏

## 4.2. 文件扩展名

| 扩展名                            | 解释                                                          |
| ------------------------------ | ----------------------------------------------------------- |
| .c                             | C source code which must be preprocessed<br>使用C语言编译器        |
| .C .cc .cp .cpp .CPP .c++ .cxx | C++ source code which must be preprocessed<br>使用C++编译器      |
| .i                             | C source code which should not be preprocessed              |
| .ii                            | C++ source code which should not be preprocessed            |
| .h                             | C or C++ header file to be turned into a precompiled header |
| .H .hh                         | C++ header file to be turned into a precompiled header      |
| .s                             | Assembler code                                              |
| .S                             | Assembler code which must be preprocessed                   |
| .o                             | Object file                                                 |
| .a                             | Static library file (archive file)                          |
| .so                            | Dynamic library file (shared object)                        |

# 5. GDB

- GDB: GNU Debug
	1. 设置断点
	2. 监视变量值
	3. 但不执行
	4. 修改变量值

| 命令           | 解释                                          |
| ------------ | ------------------------------------------- |
| file         | 打开要调试的文件                                    |
| break/tbreak | 设置断点，可以是行号、函数名及地址(以\*开头)<br> tbreak: 设置临时断点 |
| run          | 执行当前调试的程序                                   |
| list         | 执行当前调试的程序                                   |
| next         | 执行一条语句但不进入函数内部                              |
| step         | 执行一条语句，是函数则进入函数内部                           |
| display      | 显示表达式的值                                     |
| print        | 临时显示表达式的值                                   |
| kill         | 中止正在调试的程序                                   |
| quit         | 退出gdb                                       |
| shell        | 不退出gdb就执行shell命令                            |
| make         | 不退出gdb就执行make                               |

# 6. make和makefile

1. makefile
	- 描述模块间的依赖关系;
	- 记录实际编译的命令的脚本；
	- 自动支持增量编译
	- 编译开源软件时一般从./configure或cmake生成
2. make
	- 根据makefile对程序进行管理和维护；
	- 判断被维护文件的时序关系
	- make的时候用普通用户(会产生很多中间文件，如果用root会导致没有删除权限)，make install可能需要root权限(把生成的文件复制到系统目录)

## 6.1. makefile格式

```makefile
target ... : prerequisites ...
command
```

1. target是一个目标文件，可以是Object File，也可以是执行文件
2. prerequisites是要生成target所需要的文件或是目标
3. command是make需要执行的命令。（可以是任意的Shell命令）
	- **命令前面必须是Tab，不能是空格;命令和目标之间不能有空行**

```makefile
hello : main.o kbd.o 
	gcc -o hello main.o kbd.o 
main.o : main.c defs.h
	cc -c main.c
kbd.o : kbd.c defs.h command.h
	cc -c kbd.c 
clean :
	rm edit main.o kbd.o 
```

赋值号两边可以有空格

```makefile
TOPDIR = ../
include $(TOPDIR)RuIes.mak
EXTRA LIBS
EXEC =
OBJS = hello.o

all: $(EXEC)
$(EXEC): $(OBJS)
$(CC) $(LDFLAGS) -o $(OBJS)

install:
$(EXEC)

clean:
-rm -f $(EXEC) *.elf *.gdb
```

./configure  
make  
make install

make uninstall  
make clean  
make distclean 回到刚刚解压的状态

## 6.2. makefile执行次序

1. make会在当前目录下找名字叫“Makefile” 或 “makefile” 的文件。
2. 查找文件中的第一个目标文件（target），举例中的hello
3. 如果hello文件不存在，或是hello所依赖的文件修改时间要 比hello新，就会执行后面所定义的命令来生成hello文件。
4. 如果hello所依赖的.o文件不存在，那么make会在当前文 件中找目标为.o文件的依赖性，如果找到则再根据那一个 规则生成.o文件。（类似一个堆栈的过程）
5. make根据.o文件的规则生成 .o 文件，然后再用 .o 文件生 成hello文件。

## 6.3. 作用

1. 定义整个工程的编译规则
	- 一个工程中的源文件不计数，其按类型、功能、模块 分别放在若干个目录中，makefile定义了一系列的规 则来指定，哪些文件需要先编译，哪些文件需要后编 译，哪些文件需要重新编译，甚至于进行更复杂的功 能操作 。 
2. 自动化编译
	- 只需要一个make命令，整个工程完全自动编译 ； make是一个命令工具，是一个解释makefile中指令的 命令工具；

## 6.4. 命令

- make命令格式：make \[-f Makefile] \[option] \[target]

## 6.5. 伪目标

clean,install...

1. 不是一个文件，只是标签，所以 make无法生成它的依赖关系和决定它是否要执行，只 能通过显示地指明这个“目标”才能让其生效
2. “伪目标”的取名不能和文件名重名
3. 为了避免和文件重名的这种情况，可以使用一个特殊 的标记“.PHONY”来显示地指明一个目标是“伪目标 ”，向make说明，不管是否有这个文件，这个目标就 是“伪目标”
4. 伪目标一般没有依赖的文件，但也可以为伪目标指定 所依赖的文件。
5. 伪目标同样可以作为“默认目标”，只要将其放在第一个

## 6.6. 多目标

当多个目标同时依赖于一个文件，并且其生成的命令大体类 似，可以使用一个自动化变量“$@”表示着目前规则中所有 的目标的集合。

```makefile
bigoutput littleoutput : text.g
	generate text.g -$(subst output,,$@) > $@ 
```

等价于：

```
bigoutput : text.g
	generate text.g -big > bigoutput
littleoutput : text.g
	generate text.g -little > littleoutput
```

## 6.7. 预定义变量

1. $< 第一个依赖文件的名称
2. $? 所有的依赖文件，以空格分开，这些依赖文件的修改日期比目标的创建日期晚
3. $+ 所有的依赖文件，以空格分开，并以出现的先后为序，可能包含重复的依赖文件
4. $^ 所有的依赖文件，以空格分开，不包含重复的依赖文件
5. $* 不包括扩展名的目标文件名称
6. $@目标的完整名称
7. $%如果目标是归档成员，则该变量表示目标的归档成员名称

## 6.8. 多目标扩展

```
<targets ...>: <target-pattern>: <prereq-patterns ...>
	<commands>
```

```makefile
objects = foo.o bar.o

all: $(objects)
$(objects): %.o: %.c
	$(CC) -c $(CFLAGS) $< -o $@
```

等价于：

```makefile
foo.o : foo.c
	$(CC) -c $(CFLAGS) foo.c -o foo.o
bar.o : bar.c
	$(CC) -c $(CFLAGS) bar.c -o bar.o
```

1. 目标从$object中获取
2. “%.o”表明要所有以“.o”结尾的目标，即“foo.o bar.o”，就是变量 $object集合的模式
3. 依赖模式“%.c”则取模式“%.o”的“%”，也就是“foo bar”，并为其 加下“.c”的后缀，于是依赖的目标就是“foo.c bar.c

## 6.9. 函数

- 调用语法
	- `$(<function> <arguments>)`
	- `${<function> <arguments>}`

