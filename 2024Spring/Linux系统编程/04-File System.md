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
title: 04-File System
date:  2024-04-15 10:04
modified:  2024-04-15 10:04
---

# 文件系统

1. 指特定的文件格式
2. 指按特定格式进行了“格式化”的一块存储介质
3. 指操作系统用来管理文件系统以及对文件进行操作的机制以及实现


# 👍文件类型和结构

- 文件类型
	1. regular file
	2. character special file，字符设备文件
	3. block special file，块设备文件
	4. fifo，管道：没有文件名的文件
	5. socket：网络接口
	6. symbolic link
	7. directory
- 结构
	- Byte stream; no particular internal structure


# Virtual File system Switch(VFS)

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2024%2F04%2F15%2F10-27-12-c665556f9e0546a59447cd52982131cb-20240415102711-2fedd7.png)



- 组件
	1. super block
		- 描述文件系统的属性
		- e.g.只读，ext4
	2. i-node object
		- 描述文件，磁盘上**所有的文件都有一个唯一的inode**
	3. file object
		- **打开文件**，如果文件没有被打开，不会有file object
		- 文件对象需要释放
		- ~~标定唯一文件~~
	4. dentry object
		- 记录目录路径

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2024%2F04%2F15%2F10-36-57-db71799c0c080f4b2b107653f0779545-20240415103657-f29555.png)


