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
modified:  2024-05-16 10:05
---

# 编译内核

有很多复杂的configure

1. 配置
2. make
3. 启用内核

## 编译选项

- config模式
	1. Y：加入内核
	2. N：不加入内核
	3. M：模块形式，先留一个地址，`.ko`文件。需要功能的时候可以去加载。

## 启用新内核

- make install：把内核复制到\\boot目录下
	- <font color="#c00000">慎用</font>，一般手动做这个操作

grub的引导菜单会多一个刚编译的内核

## 初始化程序的建立

- initrd
	- 系统启动时的第一个用户态程序，没有父进程的进程

# 驱动

常见驱动的源代码集成在内核源码中

也有第三方驱动的开发，可以单独编译成模块`.ko`

编译需要内核头文件的支持

## 加载模块

系统开机时没有加载，但可以在运行的时候加载模块

e.g. U盘驱动

- 底层命令
	1. insmod：把模块装载到内核里
	2. rmmod：从内核里释放模块
- 高层命令
	1. modprobe：装载
	2. modprobe -r：释放


## 模块依赖


- lsmod：看当前已经装载到内核的模块


模块依赖：模块A引用模块B导出的符号，即模块B被模块A引用。如果要装载模块A，就要先装载模块B。

高层命令会解决依赖关系，而底层命令不会。


