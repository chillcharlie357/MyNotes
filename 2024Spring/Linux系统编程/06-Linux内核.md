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
title: 06-Linux内核
date:  2024-05-13 11:05
modified:  2024-05-13 11:05
---
# 编译内核


有很多复杂的configure

1. 配置
2. make
3. 启用内核
## 配置
- config模式
	1. Y：加入内核
	2. N：不加入内核
	3. M：模块形式，先留一个地址

## 启用新内核

- make install：把内核复制到\\boot目录下
	- <font color="#c00000">慎用</font>，一般手动做这个操作

grub的引导菜单会多一个刚编译的内核


## 初始化程序的建立

- initrd
	- 系统启动时的第一个用户态程序，没有父进程的进程
