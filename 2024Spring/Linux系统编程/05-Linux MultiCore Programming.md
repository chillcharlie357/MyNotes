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
title: 05-Linux MultiCore Programming
date:  2024-04-22 11:04
modified:  2024-04-22 11:04
---

# Linux进程

## exec和fork

1. exec：直接执行新的程序
2. fork：创建一个一样的新进程



## 进程退出方式

1. 正常退出
	1. return from main
	2. call exit 库函数（有终止处理）
	3. call \_exit 系统调用（立即退出）
	4. 线程终止
2. 异常退出
	1. call abort
	2. 被信号取消
	3. 线程被取消


exit：最终会调用_exit，但之前会有一堆终止处理程序(aexit function)

## Process resources


每个进程都有一个进程描述符

## wait & waitpid

```c
pid_t wait(int * status)
pid_t waitpid(pid_t  pid, int *status, int options)
```

- 作用
	1. 父进程等待子进程结束
	2. 回收僵尸进程
- 结果
	1. 阻塞
	2. 立即返回
	3. 出错



- waitpid
	1. 指定pid
	2. 非阻塞
	3. waitpid的pid参数
		1. \==-1：对应wait
		2. >0: 指定pid
		3. \==0：指定父进程的group
		4. <0：指定group id，等待对应组里的进程


## signal

进程之间通信