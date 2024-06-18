---
aliases: 
tags:
  - 2024_Spring_Linux系统编程
  - 课程
categories: 2024_Spring_Linux系统编程
sticky: 
thumbnail: 
cover: 
excerpt: false
mathjax: true
comment: true
title: 01-Linux Basic
date:  2024-02-26 09:02
modified:  2024-06-13 20:06
---

# 1. Linux

GPL许可证：可以随便用，但不能拿去卖钱

# 2. 安装系统

## 2.1. 硬盘分区

## 2.2. 硬盘组织方式

- 硬盘组织方式：决定分区在磁盘上怎么组织
	1. MBR: Master Boot Record
		1. 只能有4个主分区，1个主分区可以变成4个逻辑分区$
		2. 最大支持4T
	2. GPT: GUID Partition Table Scheme
		1. 会浪费空间
		2. 支持4T以上

## 2.3. 文件系统

- 文件系统
	- OS中负责存取和管理文件的部分，决定数据在分区里怎么放
	- Linux中常见：EXT4
	- Windows：NTFS

每个系统对自己的文件系统支持最好

## 2.4. 统一目录结构

- 统一目录结构
	- /

## 2.5. 引导

- Linux常见引导程序：GRUB

# 3. 安装软件

1. 从源码安装，make/cmake
	1. cmake： 用于生成makefile
	2. make：编译
	3. make install：从Makefile中读取指令，将编译好的文件复制到指定的安装目录中
2. 安装包

tar：打包  
gz：压缩，只压缩单个文件

# 4. 👍文件类型

Linux下的文件类型：

1. regular file
	- 普通文件
	- text, code data, video;
	- 没有特定的内部结构
2. directory
	- 目录
	- 会存放该目录下的文件列表
3. character special file
	- 字符设备
	- /dev
4. block special file
	- 块设备
	- 位于/dev目录
5. socket
	- 网络接口
6. symbolic link
	- 符号链接
7. pipe
	- 管道文件
	- FIFO

*字符设备和块设备的驱动不同*

- hard link只能给regular file创建，不能跨分区和文件系统
	- 硬链接依赖于inode号（索引节点号），而在不同的文件系统中，inode号是重新计算的
	- 不同的磁盘分区有独立的文件系统

[Hard links and soft links in Linux explained | Enable Sysadmin](https://www.redhat.com/sysadmin/linking-linux-explained)

# 5. 目录结构

[Filesystem Hierarchy Standard - Wikipedia](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard)

- unified file system
	- 从/开始的树状结构

- /home：普通用户目录
- /root：root用户目录
- /mnt：插入新的存储设备会在/mnt下创建新文件夹，并挂在在这个文件夹
- /dev：设备文件
	- 包含实际设备和虚拟设备（只有驱动，没有实际的硬件）
	- 例：随机数发生器，没有实际硬件
- /bin, /sbin：系统自带的可执行文件
- /usr：系统资源
- /etc：系统和应用的配置文件
- /var, /proc：虚拟文件，反应当前内核状态

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2024%2F03%2F07%2F10-41-13-28d161e5245d53022c87a8dffbb8eaed-20240307104112-533a50.png)

# 6. 👍基本命令

1. ls：list the contents of directories
	- -l：长列表格式显示
	- -a：显示隐藏文件
	- -R：递归显示子目录
2. cd：change directory
3. pwd：print work directory
4. mkdir: make directory 
5. rmdir：remove a empty directory
6. touch：只修改文件的更新时间为当前时间
7. cp：copy files
8. mv：move and rename files
9. ln：link files
	1. hard link：一个文件有两个名字，需要文件系统支持，不能跨分区
	2. symbolic link：快捷方式
10. rm: remove files
11. cat: print file contents
12. more/less: display files page by page

# 7. 文件权限

## 7.1. 类型

- Access Levels
	1. User: The user that created the file
	2. Group: All users in the group that owns the file
	3. Others: All others
- Permissions
	1. **R**ead
	2. **W**rite
	3. E**x**ecute: Excute file ad program or use directory as active directory

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2024%2F03%2F07%2F11-27-44-4efaed2fe509f07bcecd30503d688e88-20240307112743-70a833.png)

## 7.2. 修改

- chmod
	1. 三位八进制数
	2. +/-/= [rwx]

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2024%2F03%2F07%2F11-29-37-cc244da0d71b754913c581425a601483-20240307112936-b42f9e.png)

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2024%2F03%2F07%2F11-30-52-3fe0363c3865594780d5176f64dd7b0c-20240307113051-0ea01f.png)

- 默认权限
	- file -rw-r--r-- 644
	- directory drwxr-xr-x 755

# 8. 进程

## 8.1. 概述

进程是一个正在执行的程序实例。由执行程序、它的当前值、状态信息以及通过操作系统管理此进程执行情况的资源组成。

- Linux中每个进程都有父进程(除了init/init.rb)，形成<span style="background:rgba(3, 135, 102, 0.2)">树状结构</span>
	- init是kernel启动的第一个用户进程，PID 1
	- shell下起一个任务，父进程是shell
	- 图形界面下启一个程序，父进程是图形界面
- 结束进程
	1. 自己退出
	2. 系统信号强制退出

## 8.2. 常用命令

1. ps: report process status
2. pstree: display a tree of processes
3. jobs, fg, bg, <\ctrl-z>: job controlling
4. <span style="background:rgba(3, 135, 102, 0.2)">kill: Send </span><span style="background:rgba(3, 135, 102, 0.2)">the processes</span> identified by PID or JOBSPEC the <span style="background:rgba(3, 135, 102, 0.2)">signal</span> named by  
    SIGSPEC or SIGNUM.
	1. `kill {{process_id}}`，默认发送SIGTERM，终止进程
	2. `kill -{{9|KILL}} {{process_id}}`，发送SIGTERM
	3. `kill -{{17|STOP}} {{process_id}}`，停止进程，直到收到SIGCONT信号
5. nohup: run a command, ignoring hangup signals
6. nice, renice: 调整进程优先级
	- nice: 修改默认优先级
	- renice: 修改已经运行的进程优先级
7. top: display top CUP processes
8. 列出文件: ls
9. 创建特殊文件: mkdir
10. 文件操作:cp, mv, rm
11. 修改文件: chmod, chown, chgrp, touch
12. 查找文件: locate, find
	- find / -name "\*.txt "
	- find / -type f -exec ls -lh {} \\;
13. 字符串匹配: grep, egrep
	- grep "{{search_pattern}}" {{path/to/file}}
14. 其他: who, whoami, passwd, uname
	- who: 用于显示当前登录到系统的所有用户的信息

# 9. 系统层次

 上层调用下层

- 系统调用是Linux中内核和用户态程序的分界线
	- Windows下叫做Windows API

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2024%2F03%2F11%2F10-34-10-6b773cd048cbdbb8d0578b569a00abbf-20240311103409-abfba0.png)

# 10. 重定向

- 标准输入，标准输出，标准错误
	- shell提供的功能，不是命令提供
	- 对应文件描述符：0,1,2
	- C语言变量：stdin, stdout, stderr

```shell
kill –HUP 1234 > killout.txt 2> killerr.txt
kill –HUP 1234 > killout.txt 2>& 1
```

# 11. 管道

一个进程的输出作为后一个进程都输入

- 例子
	1. `ls | wc -l`
	2. `ls -lF | grep ^d` 正则表达式搜索

# 12. 环境变量

- 操作环境的参数
- 查看和设置环境变量
	- echo \$PATH
	- **env**
	- set

PATH里的目录下的可执行文件都能直接在shell中调用

# 13. 高级命令和正则表达式

1. find
	- `find {{root_path}} -name "{{*.ext}}"`
	- find / -type f -exec ls -lh {} \\;
2. grep
	- 在文件里查找字符串
	- `grep "{{search_pattern}}" {{path/to/file}}`
3. sed
	- 可以用来替换
	- `sed 's/apple/mango/g'`

