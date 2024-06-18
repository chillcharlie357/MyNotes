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
title: 06-Linux内核
date:  2024-05-13 11:05
modified:  2024-06-18 10:06
---

# 1. 编译内核

有很多复杂的configure

1. 配置
2. make
3. 启用内核

## 1.1. 编译选项

- config模式
	1. Y：加入内核
	2. N：不加入内核
	3. M：模块形式，先留一个地址，`.ko`文件。需要功能的时候可以去加载。

## 1.2. 启用新内核

- make install：把内核复制到\\boot目录下
	- <font color="#c00000">慎用</font>，一般手动做这个操作

grub的引导菜单会多一个刚编译的内核

## 1.3. 初始化程序的建立

- initrd
	- 系统启动时的第一个用户态程序，没有父进程的进程

# 2. 驱动

常见驱动的源代码集成在内核源码中

也有第三方驱动的开发，可以单独编译成模块`.ko`

编译需要内核头文件的支持

## 2.1. 加载模块

系统开机时没有加载，但可以在运行的时候加载模块

e.g. U盘驱动

- 底层命令
	1. <font color="#c00000">insmod</font>：**把模块装载到内核里**
	2. rmmod：从内核里释放模块
- 高层命令
	1. modprobe：装载
	2. modprobe -r：释放

## 2.2. 模块依赖

- lsmod：看当前已经装载到内核的模块
	- 和`cat /proc/modules`等价
- modinfo
- moddep

模块依赖：模块A引用模块B导出的符号，即模块B被模块A引用。如果要装载模块A，就要先装载模块B。

高层命令会解决依赖关系，而底层命令不会。

## 2.3. 模块通信

1. 共享变量
2. 调用函数

## 2.4. 👍Linux内核模块与应用程序模块的区别

|     | C      | Linux内核模块       |
| --- | ------ | --------------- |
| 运行  | 用户空间   | 内核空间            |
| 入口  | main() | module_init()指定 |
| 出口  | 无      | module_exit()指定 |
| 运行  | 直接指定   | insmod          |
| 调试  | gdb    | kdbug,kdb,kgdb等 |

1. 内核模块其实并不存在入口出口，init做初始化，exit做释放，加载后一直在内存里待命
2. 内核模块真正执行的时候是用户态程序调用相关功能
	- e.g. 内核加载了一段打印机的驱动，一直在内存里待命，在用户态程序（如Word）里点了打印，这段驱动才会被调用（通过系统调用）
	- 要考虑并行会出现的问题，可能有多个用户同时打印

## 2.5. 内核程序注意事项

1. 不能调用C库来开发驱动程序
2. 没有内存保护机制
3. 小内核栈
	- 内核一般不用递归否则会占用大量内存空间
4. 并发考虑
5. 内核代码/Shell脚本没有类型浮点支持

```c
#include <linux/kernel.h>
#include <linux/module.h>
#include <linux/init.h>
static int __init hello_init(void)
{
	printk(KERN_INFO "Hello world\n");
	return 0;
}
static void __exit hello_exit(void)
{
	printk(KERN_INFO "Goodbye world\n");
}
module_init(hello_init);
module_exit(hello_exit);
```

## 2.6. 编译

内核Makefile和一般的不一样，而且经常更新

## 2.7. 模块参数传递

- 传递方式
	1. 参数在模块加载时传递
		- shell: `insmod hello.ko test=2`
	2. 参数需要使用module_param宏来声明
		- `module_param(变量名称，类型, 访问许可掩码)`
- 支持参数类型
	- Byte, short, ushort, int, uint, long, ulong, bool, charp
	- Array (module_param_array(name, type, nump, perm))

```c
#include <linux/kernel.h>
#include <linux/module.h>
#include <linux/init.h>
#include <linux/moduleparam.h>

static int test;
module_param(test, int, 0644);

static int __init hello_init(void)
{
	printk(KERN_INFO “Hello world test=%d \n” , test);
	return 0;
}
static void __exit hello_exit(void)
{
	printk(KERN_INFO "Goodbye world\n");
}

MODULE_LICENSE("GPL");
MODULE_DESCRIPTION("Test");
MODULE_AUTHOR("xxx");
module_init(hello_init);
module_exit(hello_exit);
```

# 3. 字符型设备

## 3.1. 驱动程序的初始化加载过程

1. 申请设备号
2. 定义文件操作结构体 file_operations
3. 创建并初始化定义结构体 cdev
4. 将cdev注册到系统，并和对应的设备号绑定
5. 在/dev文件系统中用mknod创建设备文件，并将该文件绑定到设备号上

## 3.2. 主设备号和次设备号

- 一个字符设备或者块设备都有一个主设备号和次设备号。
- 主设备号和次设备号统称为设备号。
- 主设备号用来表示一个**特定的驱动程序**。
- 次设备号用来表示**使用该驱动程序的各设备**。

## 3.3. 应用程序调用驱动

1. 加载设备驱动：应用程序需要加载一个驱动模块（.ko文件），这个过程会生成一个设备文件，该文件代表了驱动程序控制的硬件设备或本地设备
2. 打开设备文件：应用程序使用系统提供的函数（如`open()`）来打开设备文件。这些函数最终会通过内核转发到相应的驱动函数。每个设备文件都有一个对应的inode结构体，包含了设备的主次设备号，是设备的唯一标识
3. 执行系统调用：应用程序通过执行系统调用（如`read()`, `write()`, `ioctl()`等）与设备驱动进行交互。这些调用最终会通过内核转发到相应的驱动函数