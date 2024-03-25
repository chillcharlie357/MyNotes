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

# Programming Language

- High-level Language
	- C/C++, Java, Fortran…
	- ELF binary format
	- binary code: 本地二进制码，直接对应机器指令
- Script 
	- Shell: sh/bash, csh, ksh
	- Perl, Python, tcl/tk, sed, awk

# 编译

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2024%2F03%2F25%2F11-15-22-efb7064e12a9cbfd0fdedef49bcc392a-20240325111521-19928c.png)

- 预处理
	- 把预处理指令处理掉（`#include`, `#if`,`#define`...）
	- `#include`把目标文件粘贴到include的位置
	- `#define`字符串替换

- 链接：保证可执行文件里的二进制码是完整的

