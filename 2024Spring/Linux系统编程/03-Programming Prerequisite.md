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
modified:  2024-03-25 11:03
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

# GCC options

gcc的参数和llvm差不多

- 只编译: `gcc -c test.c -o test.obj`， 输出`.obj`
- 只链接`gcc test.obj -o test`，输出可执行文件
- 编译链接一起做：`gcc test.c -o test`，