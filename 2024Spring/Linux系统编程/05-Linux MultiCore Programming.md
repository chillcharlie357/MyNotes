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

### 信号

SIGKILL：终止，不能被捕获或忽略
SIGINT：终端中断符
SIGTERM：终止（kill发出的默认系统终止信号），可以改

### 可靠性

- 信号可靠性
	1. 连续重复信号能不能收到
		1. 可能会丢失，SIG对应的int值较大对应早期Linux版本不可靠，int值较小对应早期版本;后期版本有可靠机制
	2. 阻塞信号
	3. 复位机制



### 发信号

1. kill
2. raise
3. alarm: set an alarm clock for delivery of a signal
	1. 每个进程只能有一个闹钟
	2. 可以用来做超时处理
4. pause: wait for a signal
	1. 挂起，等到有信号来才执行
	2. e.g. CTRL+Z的实现


### 可靠信号

信号集

给一个信号注册一个结构体，而不是直接注册处理函数

- sigprocmask
