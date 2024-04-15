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


# 符号链接

- Hard Link
	1. 不同文件共用一个inode
		- 两个文件没有主次关系删掉一个没有影响
	2. 不能跨文件系统/分区
	3. 对应**系统调用link**
- Symbolic link
	1. 存储被链接文件的文件名（而不是inode）实现链接
	2. 可以跨文件系统
	3. 对应**系统调用symlink**


- ls -l查看链接数目
	- ![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2024%2F04%2F15%2F11-17-25-fc0746aebea93640a4b72174da722b7c-20240415111724-a9fb6d.png)



# 系统调用

- 都以C函数的形式出现
- 系统调用
	- Linux内核的对外接口; 用户程序和内核之间唯一的接口; 提供最小接口
- 库函数
	- 依赖于系统调用; 提供较复杂功能
	- 例：标准I/O库


## Basic I/O System Calls


### File Descriptor

用户态程序访问文件最底层的句柄，再往下就是内核。

1. 是一个int值
	- `unistd.h`：STDIN_FILENO (0), STDOUT_FILENO (1), STDERR_FILENO (2)
2. 系统调用的返回值
	- open,read,write...


e.g.使用系统调用读写文件
```c
#include <fcntl.h>
main(){
	int fd, nread;
	char buf[1024];
	/*open file “data” for reading */
	fd = open(“data”, O_RDONLY);
	/* read in the data */
	nread = read(fd, buf, 1024);
	/* close the file */
	close(fd);
}
```


### opne/creat function


```c
#include <sys/types.h> 
#include <sys/stat.h> 
#include <fcntl.h> 
int open(const char *pathname, int flags);
int open(const char *pathname, int flags, mode_t mode); //可变参数，不是函数重载
int creat(const char *pathname, mode_t mode);
//Return: a new file descriptor if success; -1 if failure
```

- flags: `O_RDONLY`, `O_WRONLY`, `O_EDWR`
- `creat`：`open` with flags`O_CREAT|O_WRONLY|O_TRUNC`



### close 

```c
#include <unistd.h>
int close(int fd);
//Return: 0 if success; -1 if failure
```


### read/write

read from a file descriptor
```c
#include <unistd.h>
ssize_t read(int fd, void *buf, size_t count);
//返回值: 读到的字节数，若已到文件尾为0，若出错为-1
```

write to a file descriptor
```c
#include <unistd.h>
ssize_t write(int fd, const void *buf, size_t count);
//返回值: 若成功为已写的字节数，若出错为-1
```

