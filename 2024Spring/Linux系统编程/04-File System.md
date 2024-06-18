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
title: 04-File System
date:  2024-04-15 10:04
modified:  2024-06-18 14:06
---

# 1. 文件系统

1. 指特定的文件格式
2. 指按特定格式进行了“格式化”的一块存储介质
3. 指操作系统用来管理文件系统以及对文件进行操作的机制以及实现

# 2. 👍文件类型和结构

- 文件类型
	1. regular file
	2. character special file，字符设备文件
	3. block special file，块设备文件
	4. fifo/pipe：管道文件，没有文件名的文件
	5. socket：网络接口
	6. symbolic link
	7. directory
- 结构
	- Byte stream; no particular internal structure

# 3. Virtual File system Switch(VFS)

- 作用
	1. 抽象接口，上层程序可以通过统一的系统调用访问不同文件系统
	2. 多文件系统共存
	3. 维护文件和目录的树状结构
	4. 文件权限控制
	5. 文件缓存和读写优化，VFS与内核的内存管理子系统协作，支持文件数据的缓存和页缓存机制
	6. 网络文件支持

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2024%2F04%2F15%2F10-27-12-c665556f9e0546a59447cd52982131cb-20240415102711-2fedd7.png)

**位于内核态**，比系统调用更加底层。

- 组件
	1. super block
		- 描述文件系统的属性
		- e.g.只读，ext4
	2. i-node object
		- 描述文件，磁盘上**所有的文件都有一个唯一的inode**
		- **记录真正的文件，文件存储在磁盘上时是按照索引号访问文件的，软链接是不同的文件，但是硬链接是相同的inode号，同一个文件。**
	3. file object
		- **打开文件**，如果文件没有被打开，不会有file object
		- 文件对象需要释放
		- ~~标定唯一文件~~
	4. dentry object
		- 记录目录路径

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2024%2F04%2F15%2F10-36-57-db71799c0c080f4b2b107653f0779545-20240415103657-f29555.png)

# 4. 硬链接和符号链接

- Hard Link
	1. 不同文件共用一个inode
		- 两个文件没有主次关系删掉一个没有影响
	2. 不能跨文件系统/分区
	3. 只能对regular file创建
	4. 对应**系统调用link**
	5. `ln {{/path/to/file}} {{path/to/hardlink}}`
- Symbolic link
	1. 存储被链接文件的文件名（而不是inode）实现链接
	2. 可以跨文件系统
	3. 可以对任何文件类型创建
	4. 对应**系统调用symlink**
	5. `ln -s {{/path/to/file_or_directory}} {{path/to/symlink}}`

- ls -l查看链接数目
	- ![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2024%2F04%2F15%2F11-17-25-fc0746aebea93640a4b72174da722b7c-20240415111724-a9fb6d.png)

# 5. System Calls

- 都以**C函数**的形式出现
- **系统调用**
	1. Linux内核的对外接口;
	2. 用户程序和内核之间唯一的接口;
	3. 提供最小接口
- **库函数**
	1. 依赖于系统调用
	2. 提供较复杂功能
	3. 例：标准I/O库

- Basic I/O System Calls
	1. 没有缓冲区（buffer）
	2. 使用File descriptor

## 5.1. File Descriptor

**用户态程序访问文件最底层的句柄**，再往下就是内核。  
可以理解成下标，数组在内核态（如果存在）

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

## 5.2. opne/creat function

```c
#include <sys/types.h> 
#include <sys/stat.h> 
#include <fcntl.h> 
int open(const char *pathname, int flags);
int open(const char *pathname, int flags, mode_t mode); //可变参数，不是函数重载
int creat(const char *pathname, mode_t mode);
//Return: a new file descriptor if success; -1 if failure
```

- flags: `O_RDONLY`, `O_WRONLY`, `O_RDWR`，`O_TRUNC:`， `O_APPEND`
- mode: 指定权限
	- ![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2024%2F06%2F15%2F19-58-41-b7c02ff2b68a05537120b877293752e3-20240615195840-fe85a4.png)

- `creat`：`open` with flags`O_CREAT|O_WRONLY|O_TRUNC`

## 5.3. close

```c
#include <unistd.h>
int close(int fd);
//Return: 0 if success; -1 if failure
```

## 5.4. read/write

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

```c
while ((n = read(STDIN_FILENO, buf, BUFSIZE)) > 0)
if (write(STDOUT_FILENO, buf, n) != n)
	err_sys(“write error”);
if (n<0)
	err_sys(“read error”);
```

## 5.5. seek

- 设置read/write的偏移量

```c
#include <sys/types.h>
#include <unistd.h>
off_t lseek(int fildes, off_t offset, int whence);
//Return: the resulting offset location if success; -1 if failure)
```

- whence: seek从哪里开始加偏移量
	1. SEEK_SET: the offset is set to “offset” bytes
	2. SEEK_CUR: the offset is set to its current location plus “offset” bytes
	3. SEEK_END: the offset if set to the size of the file plus “offset“ bytes

## 5.6. dup/dup2 Function

- 复制文件描述符

```c
#include <unistd.h>
int dup(int oldfd);
int dup2(int oldfd, int newfd);
//Return: the new file descriptor if success; -1 if failure)
```

- e.g.重定向的实现

```c
int fd = open(...)
dup2(fd,1)
```

## 5.7. fcntl Function

- 控制文件描述符

```c
#include <unistd.h>
#include <fcntl.h>
int fcntl(int fd, int cmd);
int fcntl(int fd, int cmd, long arg);
int fcntl(int fd, int cmd, struct flock *lock);
//返回值: 若成功则返回值依赖于cmd，若出错为-1
```

- cmd取值
	1. F_DUPFD: **Duplicate** a file descriptor
	2. F_GETFD/F_SETFD: Get/set the **file descriptor’s close-on-exec flag**
		- close-on-exec flag: 调用exec时文件描述符会不会关闭
		- 为解决fork子进程执行其他任务(exec等)导致父进程的文件描述符被复制到子进程中，使得对应文件描述符无法被之后需要的进程获取。
	3. F_GETFL/F_SETFL: Get/set the **file descriptor’s flags**
		- open/creat中的flags 参数
	4. F_GETOWN/F_SETOWN: **Manage I/O availability signals**
		- 可以向文件发几个信号
		- 获得或设置当前文件描述符会接受SIGIO和SIGURG信号的进程或进程组编号
	5. F_GETLK/F_SETLK/F_SETLKW: Get/set the **file lock**
		- S_SETLKW同S_SETLK，但是在锁无法设置时会阻塞等待任务完成。

```c
//file:fcntl
int main()
{
	pid_t pid;
	fd =open("test.txt",0_RDWR|0_APPEND);if(fd ==-1)
	//printf("open err/n");
	printf("fd =%d",fd);
	printf("fork!/n");
	fcntl(fd,F_SETFD,1);
	char *s="0000000000000000000";
	pid =fork();
	if(pid ==θ)
	execl("ass","./ass",&fd,NULL);
	wait(NULL);
	write(fd,s,strlen(s));
	close(fd);
	return θ;
}

//file:ass.c
int main(int argc,char *argv[]){
	int fd;
	printf("argc =%d“,argc);
	fd =*argv[1];
	printf("fd =%d",fd);
	char *s ="zzzzzzzzzzzzzzzzzzz";
	write(fd,(void *)s,strlen(s));
	close(fd);
	return θ;
}

```

## 5.8. ioctl function

- 控制设备

```c
#include <sys/ioctl.h>
int ioctl(int d, int request, ...);
```

- `d`：文件描述符
- `request`：也是命令，但是是自定义的

# 6. Standard I/O Library

## 6.1. File Stream

使用用户态的**结构体**`FILE`，而不是文件描述符。`FILE`结构体内部保存文件描述符。

有预先定义好的stdin, stdout, stderr

- buffer
	1. Full buffer
	2. Line buffer
	3. No buffer

## 6.2. Stream Buffering Operations

- buffer类型
	1. block buffered (fully buffered)
	2. line buffered
	3. unbuffered

```c
#include <stdio.h>

void setbuf(FILE *stream, char *buf);

int setvbuf(FILE *stream, char *buf, int mode, size_t size);
```

- type:
	- `_IOFBF`(满缓冲）
	- `_IOLBF`(行缓冲）
	- `_IONBF`(无缓冲）

## 6.3. Stream open/close

- 创建/销毁stream

```c
#include <stdio.h>
FILE *fopen(const char *filename, const char *mode);
int fclose(FILE *stream);
```

- mode
	1. r: reading
	2. w: Truncate file to zero length or create text file for writing.
	3. a: appending
	4. r+: reading and writing
	5. w+: Open for reading and writing. The file is created if it does not exist, otherwise it is truncated.
	6. a+: Open for reading and appending. The file is created if does not exist.

## 6.4. Input of a character

1. `getc`是宏定义，速度比`fgetc`快
2. `fgetc`
3. `getchar
4. `ungetc`：把字符放到stream中
5. `ferror(FILE *stream)`: 检查指定的文件流 `stream` 是否发生了错误。
6. `feof(FILE *stream)`: stream是否已经到达EOF
7. `clearerr(FILE *stream)`：清除指定文件流 `stream` 的错误标志和 EOF 标志。

```c
#include <stdio.h>
int getc(FILE *fp);
int fgetc(FILE *fp);
int getchar(void);

//Result: Reads the next character from a stream and returns it as an unsigned char cast to an int, or EOF on end of file or error.
```

## 6.5. Output of a Character

```c
#include <stdio.h>
int putc(int c, FILE *fp);
int fputc(int c, FILE *fp);
int putchar(int c);
//Return: the character if success; -1 if failure
```

## 6.6. Input of a Line of String

```c
#include <stdio.h>
char *fgets(char *s, int size, FILE *stream);
char *gets(char *s); //not recommended.
```

fgets最多读取size-1个字符到stream，读到EOF或换行符停止，结尾会存一个`\0`

## 6.7. Output of a Line of String

```c
#include <stdio.h>
int fputs(const char *s, FILE *stream);
int puts(const char *s
```

## 6.8. Binary Stream Input/Output

```c
#include <stdio.h>
size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream);
size_t fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream);
//Return: the number of a items successfully read or written.
```

fwrite: 向磁盘写入数组，数组首地址ptr，数组每个格子大小size, 数组元素个数nmemb

- 写一个数组：

```c
float data[10];
if ( fwrite(&data[2], sizeof(float), 4, fp) != 4 )
	err_sys(“fwrite error”);
```

- 写一个结构体

```C
struct {
	short count; 
	long total; 
	char name[NAMESIZE];
}item;
if ( fwrite(&item, sizeof(item), 1, fp) != 1)
	err_sys(“fwrite error”);
```

[c - correct way to use fwrite and fread - Stack Overflow](https://stackoverflow.com/questions/43348672/correct-way-to-use-fwrite-and-fread)

## 6.9. Formatted I/O

```c
#include <stdio.h>
int scanf(const char *format, ...);
int fscanf(FILE *stream, const char *format, ...);
int sscanf(const char *str, const char *format, ...);
```

使用`fgets`, 然后解析字符串

---

```c
#include <stdio.h>
int printf(const char *format, ...);
int fprintf(FILE *stream, const char *format, ...);
int sprintf(char *str, const char *format, ...);
```

## 6.10. Reposition a stream

```C
#include <stdio.h>
int fseek(FILE *stream, long int offset, int whence);
long ftell(FILE *stream);
void rewind(FILE *stream);
```

```C
#include <stdio.h>
int fgetpos(FILE *fp, fpos_t *pos);
int fsetpos(FILE *fp, const fpos_t *pos);
```

## 6.11. Flush a stream

刷写文件流，把流里的数据立刻写入文件

```c
#include <stdio.h>
int fflush(FILE *stream);
```

## 6.12. Stream and File Descriptor

- 确定流使用的底层文件描述符

```c
#include <stdio.h>
int fileno(FILE *fp);
```

- 根据已打开文件描述符创建一个流

```c
#include <stdio.h>
FILE *fdopen(int fildes, const char *mode);
```

## 6.13. Temporary File

- 为临时文件创建名称
	- 返回值：指向唯一路径名的指针

```c
#include <stdio.h>
char *tmpnam(char *s);
```

- 创建临时文件
	- 返回值：如果成功返回文件指针，否则为NULL

```c
#include <stdio.h>
FILE *tmpfile(void);
```

# 7. Advanced System Calls

- Handling file attributes
	- stat/fstat/lstat, ...
- Handling directory

## 7.1. stat/fstat/lstat

获取文件状态。

```C
#include <sys/types.h>
#include <sys/stat.h>
#include <unistd.h>
int stat(const char *filename, struct stat *buf);
int fstat(int filedes, struct stat *buf);
int lstat(const char *file_name, struct stat *buf);
//Return: 0 if success; -1 if failure
```

- <span style="background:rgba(3, 135, 102, 0.2)">lstate和stat的区别</span>在于对符号链接的处理：
	1. `stat` 会跟随符号链接并获取链接目标的文件状态信息
	2. `lstat` 则获取**链接本身**的文件状态信息。当你需要确定一个文件是不是符号链接，或者你需要获取符号链接本身的信息（比如权限、所有者等），这时应该使用 `lstat`。

---

```c
struct stat {
	mode_t st_mode; /*file type & mode*/
	ino_t st_ino; /*inode number (serial number)*/
	dev_t st_rdev; /*device number (file system)*/ 
	nlink_t st_nlink; /*link count*/
	uid_t st_uid; /*user ID of owner*/
	gid_t st_gid; /*group ID of owner*/
	off_t st_size; /*size of file, in bytes*/
	time_t st_atime; /*time of last access*/
	time_t st_mtime; /*time of last modification*/
	time_t st_ctime; /*time of last file status change*/
	long st_blksize; /*Optimal block size for I/O*/
	long st_blocks; /*number 512-byte blocks allocated*/
};
```

- types
	- ![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2024%2F06%2F16%2F15-44-56-d242d33e62d872278b310f72104be0f3-20240616154455-89aee9.png)
- mode
	- ![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2024%2F06%2F16%2F15-44-31-651ec3334b5d75bec36c65ffcd3cc500-20240616154430-2690d8.png)

## 7.2. umask

为进程设置文件存取权限屏蔽字，并返回以前的值。  
定义了文件系统创建文件和目录时**默认应该屏蔽掉的权限位**

```C
#include <sys/types.h>
#include <sys/stat.h>
mode_t umask(mode_t mask);
```

```c
#include <sys/stat.h>
#include <stdio.h>

int main() {
    struct stat statbuf;
    if (stat("example.txt", &statbuf) == 0) {
        printf("File size: %ld bytes\n", statbuf.st_size);
    } else {
        perror("stat failed");
    }

    // 设置umask
    umask(S_IWGRP | S_IWOTH); // 屏蔽组和其他用户的写权限

    // 创建文件，权限将自动应用umask
    FILE *fp = fopen("newfile.txt", "w");
    if (fp != NULL) {
        fprintf(fp, "Hello, World!");
        fclose(fp);
    } else {
        perror("fopen failed");
    }

    return 0;
}
```

## 7.3. directory

- 数据结构
	1. DIR：文件夹
	2. struct dirent：文件夹中的目录项

- DIR

```c
//in <dirent.h>
//The data type of directory stream objects
typedef struct __dirstream DIR;
```

- struct dirent

```c
//in <dirent.h>
//Directory item
ino_t d_ino; /* inode number */
char d_name[NAME_MAX + 1]; /* file name */
```

---

- 操作:

```c
#include <sys/types.h>
#include <dirent.h>
DIR *opendir(const char *name);
int closedir(DIR *dir);
struct dirent *readdir(DIR *dir);
off_t telldir(DIR *dir);
void seekdir(DIR *dir, off_t offset);
```

---

```c
DIR *dp;
struct dirent *entry;
if ( (dp = opendir(dir)) == NULL )
	err_sys(…);
while ( (entry = readdir(dp)) != NULL ) {
	lstat(entry->d_name, &statbuf);
	if ( S_ISDIR(statbuf.st_mode) ) 
		…
	else
		…
}
closedir(dp);
```

# 8. File lock

作用：几个进程同时操作一个文件

## 8.1. 分类

- 记录锁：往文件加锁时，是否要锁整个文件，还是只锁一部分（记录锁）。**允许只对文件特点部分上锁**。

---

- 劝告锁：
	- 检查，加锁由应用程序控制
	- 系统会给进程发信号，告诉他这个文件被上锁，但是**可以强行操作文件**
- 强制锁：
	- 检查，加锁由内核控制
	- **不允许违反锁规则访问**
	- 影响open,write,read等操作

---

- 共享锁
- 排他锁

## 8.2. fcntl

```c
#include <unistd.h>
#include <fcntl.h>
int fcntl(int fd, int cmd, struct flock *lock);
//返回值: 若成功则依赖于cmd，若出错为-1
```

```c
struct flock{
	...
	short l_type; /* Type of lock: F_RDLCK, F_WRLCK, F_UNLCK */
	short l_whence; /* How to interpret l_start: SEEK_SET, SEEK_CUR, 
	SEEK_END */
	off_t l_start; /* Starting offset for lock */
	off_t l_len; /* Number of bytes to lock */
	pid_t l_pid; /* PID of process blocking our lock (F_GETLK only) */
	...
}
```

- cmd参数
	1. F_GETLK：获得文件的封锁信息
	2. F_SETLK：对文件的某个区域封锁或解除封锁
	3. F_SETLKW：功能同F_SETLK, **wait方式**
- l_type
	1. F_RDLCK：**共享锁（Shared Lock）**,read lock
	2. F_WRLCK：**排他锁（Exclusive Lock）**,write lock
	3. F_UNLCK：**解锁（Unlock）**
- l_whence
	- 起始位置的参考点，通常是`SEEK_SET`（文件开头）、`SEEK_CUR`（当前位置）或`SEEK_END`（文件结尾）
- l_len：锁的长度，0通常表示直到文件末尾。
- l_start：锁的起始位置，可以指定为字节偏移量。
- **记录锁（Record Lock）** 的实现：
	- 通过设置 `l_start`、`l_len` 和 `l_whence` 字段来定义锁的范围

[fcntl实现强制锁和劝告锁](https://cn.linux-console.net/?p=23446#:~:text=%E5%BC%BA%E5%88%B6%E9%94%81%E6%98%AF%E6%96%87%E4%BB%B6%E9%94%81%EF%BC%8C%E5%8F%AF%E9%98%B2%E6%AD%A2%E5%85%B6%E4%BB%96%E8%BF%9B%E7%A8%8B%E8%AE%BF%E9%97%AE%E6%88%96%E4%BF%AE%E6%94%B9%E6%96%87%E4%BB%B6%EF%BC%8C%E7%9B%B4%E5%88%B0%E9%94%81%E8%A2%AB%E9%87%8A%E6%94%BE%E3%80%82%20%E5%BC%BA%E5%88%B6%E9%94%81%E7%94%B1%E5%86%85%E6%A0%B8%E8%AE%BE%E7%BD%AE%EF%BC%8C%E4%B8%8D%E8%83%BD%E8%A2%AB%E8%BF%9B%E7%A8%8B%E8%A6%86%E7%9B%96%E3%80%82%20%E5%BD%93%E6%96%87%E4%BB%B6%E5%BE%88%E5%85%B3%E9%94%AE%E5%B9%B6%E4%B8%94%E4%B8%8D%E8%83%BD%E8%A2%AB%E4%BB%BB%E4%BD%95%E5%85%B6%E4%BB%96%E8%BF%9B%E7%A8%8B%E4%BF%AE%E6%94%B9%E6%97%B6%EF%BC%8C%E5%BC%BA%E5%88%B6%E9%94%81%E5%BE%88%E6%9C%89%E7%94%A8%E3%80%82%20fcntl,%28%29%20%E5%87%BD%E6%95%B0%E8%BF%98%E7%94%A8%E4%BA%8E%E5%AF%B9%E6%96%87%E4%BB%B6%E8%AE%BE%E7%BD%AE%E5%BC%BA%E5%88%B6%E9%94%81%E5%AE%9A%E3%80%82%20F_SETLK%20%E6%A0%87%E5%BF%97%E7%94%A8%E4%BA%8E%E5%9C%A8%E6%96%87%E4%BB%B6%E4%B8%8A%E8%AE%BE%E7%BD%AE%E5%BC%BA%E5%88%B6%E9%94%81%E5%AE%9A%E3%80%82)
