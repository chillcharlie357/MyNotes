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
title: 0-Linux Basic
date:  2024-02-26 09:02
modified:  2024-03-07 10:03
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
2. 安装包

tar：打包  
gz：压缩，只压缩单个文件

# 4. 👍文件类型

LInux下的文件类型：

1. regular file
	- text, code data, video;
	- 没有特定的内部结构
2. character special file
	- 字符设备
	- /dev
3. block special file
	- 块设备
	- 位于/dev目录
4. socket
	- 网络接口
5. symbolic file
	- 符号连接
6. directory
	- 目录
	- 会存放该目录下的文件列表
7. pipe

*字符设备和块设备的驱动不同*

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

1. ls
	- -l：长列表格式显示
	- -a：显示隐藏文件
	- -R：递归显示子目录
2. cd：change directory
3. pwd：print work directory
4. mkdir: make directo 
5. rmdir：remove a empty directory
6. touch：只修改文件的更新时间为当前时间
7. cp：copy files
8. mv：move and rename files
9. ln：link files
10. rm: remove files
11. cat: print file contents
12. more/less: display files page-by page

