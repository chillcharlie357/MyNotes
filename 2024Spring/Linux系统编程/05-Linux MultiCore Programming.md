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
modified:  2024-05-06 11:05
---

# 1. Linux进程

## 1.1. exec和fork

1. exec：直接执行新的程序
2. fork：创建一个一样的新进程

## 1.2. 进程退出方式

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

## 1.3. Process resources

每个进程都有一个进程描述符

## 1.4. wait & waitpid

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

		2. > 0: 指定pid

		3. \==0：指定父进程的group
		4. <0：指定group id，等待对应组里的进程

## 1.5. signal

进程之间通信

### 1.5.1. 信号

SIGKILL：终止，不能被捕获或忽略  
SIGINT：终端中断符  
SIGTERM：终止（kill发出的默认系统终止信号），可以改

### 1.5.2. 可靠性

- 信号可靠性
	1. 连续重复信号能不能收到
		1. 可能会丢失，SIG对应的int值较大对应早期Linux版本不可靠，int值较小对应早期版本;后期版本有可靠机制
	2. 阻塞信号
	3. 复位机制

### 1.5.3. 发信号

1. kill: send signal to a process
2. raise: send a signal to the current process
3. alarm: set an alarm clock for delivery of a signal
	1. 每个进程只能有一个闹钟
	2. 可以用来做超时处理
4. pause: wait for a signal
	1. 挂起，等到有信号来才执行
	2. e.g. CTRL+Z的实现

### 1.5.4. 可靠信号

信号集

给一个信号注册一个结构体，而不是直接注册处理函数

- sigprocmask：检测或更改(或两者)进程的信号掩码
- **sigaction**：检查或修改与指定信号的关联处理动作

```c
#include <signal.h>
int sigaction(int signum, const struct sigaction *act, struct 
sigaction *oldact);
//Returned Value: 0 is success, -1 if failure)
```

struct sigaction成员：

```c
handler_t sa_handler; /* addr of signal handler, or SIG_IGN, 
or SIG_DEL */
sigset_t sa_mask; /* additional signals to block */
int sa_flags; /* signal options */
```

- `sigsuspend`：使用临时信号替代信号掩码，在捕获一个信号或发生终止该进程的信号前，进程挂起

```c
#include <signal.h>
int sigsuspend(const sigset *sigmask);
//Returned value: -1, errno is set to be EINTR
```

# 2. 共享内存

1. 共享内存是内核为进程创建的一个特殊内存段，它可连接(attach)到自己的地址空间，也可以连接到其它进程的地址空间
2. **最快的进程间通信方式**
3. 不提供任何同步功能

## 2.1. mmap/munmap

把文件/设备，映射/取消映射到内存。

```c
void* mmap(void* addr,size_t length,int prot, int flags,int fd,off_t offset)

int munmap(void* addr, size_t length)
```

- flages
	1. MAP_SHARED
	2. MAP_ANONYMOUS：忽略掉fd，虚拟了一个文件
	3. MAP_PRIVATE：只有当前进程可以写

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2024%2F04%2F29%2F10-49-59-a380e5e4559554e6c4bd764939c0322a-20240429104958-5ac065.png)

## 2.2. shared memory system calls

### 2.2.1. 申请共享内存

```c
int shmget(key_t key, int size, int flag);
```

1. key：区分共享内存。
2. flag：
	1. 0：必须找
	2. IPC_CREAT：找不到就创建
	3. IPC_EXCL：只创建不找

### 2.2.2. 共享内存地址映射

使用共享内存的时候也需要map，和文件类似:

```c
void *shmat(int shmid, void *addr, int flag);
```

1. addr
2. flag: 
	1. SHM_RND：可读可写
	2. SHM_RDONLY：只读

```c
shared_memory = shmat(shmid, (void *)0, 0);
if (shared_memory == (void *)-1) {
		fprintf(stderr, "shmat failed\n");
		exit(EXIT_FAILURE);
}

```

### 2.2.3. 共享内存解除映射

```c
int shmdt(void * addr);
```

```c
if (shmdt(shared_memory) == -1) {
	fprintf(stderr, "shmdt failed\n");
	exit(EXIT_FAILURE);
}
```

### 2.2.4. 共享内存释放

```c
int shmctl(int shmid, int cmd, struct shmid_ds *buf);
```

1. cmd：
	1. IPC_STAT：获取共享内存属性
	2. IPC_SET：为共享内存设置属性
	3. **IPC_RMID：释放**，buf参数

e.g.:

```c
if (shmctl(shmid, IPC_RMID, 0) == -1) {
	fprintf(stderr, "shmctl(IPC_RMID) failed\n");
	exit(EXIT_FAILURE);
}
```

# 3. POSIX thread

1. POSIX thread不是系统调用，是Linux下的标准库
2. Linux下可以用clone创建thread，但是比较复杂很少用

线程共享地址空间，轻量级

- pthread library
	- /usr/lib/libpthread.so, /usr/lib/libpthread.a
- pthread.h header file
	- /usr/include/pthread.h
- Compiler options
	- gcc thread.c –o thread –lpthread
	- -l: link，链接本地二进制码

```c
gcc a.c -lpthread
//libpthread.so,libpthread.a
```

## 3.1. 命名

pthread下的所有函数都以`pthread_`开头

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2024%2F04%2F29%2F11-31-51-7c6da78deef9bd2d44a457253224f8cc-20240429113150-2588f6.png)

## 3.2. 线程操作

### 3.2.1. 生命周期

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2024%2F04%2F29%2F11-40-13-c1cd41e1edb023fd1ea6491d58ea4140-20240429114012-6287c9.png)

### 3.2.2. pthread_create

创建线程

```c
#include <pthread.h>
int pthread_create(
pthread_t *thread, 
pthread_attr_t *attr, 
void *(*start_routine)(void *), 
void *arg)
```

1. thread：输出线程ID
2. attr：创建线程的时候可以传属性，默认可以传`NULL`
3. `start_routine`：线程的入口函数
4. arg

### 3.2.3. pthread_exit

结束线程

```c
void pthread_exit(void * retval);
```

可以在线程里调用`pthread_exit`，也可以在线程`main`函数`return`

### 3.2.4. pthread_join

- 等待另一个线程结束
	- pthread库并没有限制只能主线程join子线程，但是推荐

```c
int pthread_join(pthread_t th, void **thread_return);
```

1. `th`：需要等待的线程
2. `thread_return`：指向线程返回值的指针
	- 返回值是`void*`

### 3.2.5. pthread_detach

- 线程自己回收自己的资源，不和任何线程做join

```c
int pthread_detach(pthread_t th);
```

## 3.3. 线程同步

### 3.3.1. 信号量

```c
#include <semaphore.h>
int sem_init(sem_t *sem, int pshared, unsigned int 
value);
int sem_wait(sem_t *sem); //P
int sem_post(sem_t *sem); //V
int sem_destroy(sem_t *sem);// 释放
int sem_trywait(sem_t *sem); //不阻塞的P
int sem_getvalue(sem_t *sem, int *sval);
```

- sem_init
	1. sem：指向信号量指针
	2. pshared：是否共享
	3. value：初始值

### 3.3.2. 互斥量

```c
#include <pthread.h>
int pthread_mutex_init(pthread_mutex_t *mutex, const 
pthread_mutexattr_t *mutexattr);
int pthread_mutex_lock(pthread_mutex_t *mutex);
int pthread_mutex_unlock(pthread_mutex_t *mutex);
int pthread_mutex_destroy(pthread_mutex_t *mutex);
int pthread_mutex_trylock(pthread_mutex_t *mutex);
```

互斥量里只有一份资源，内部是Boolean变量。

### 3.3.3. 条件变量

 等待某个变量满足条件

#### 3.3.3.1. 初始化

1. 静态初始化`pthread_cond_t convar = PTHREAD_COND_INITIALIZER;`
2. `pthread_cond_init()`

```c
int pthread_cond_init(pthread_cond_t *cond, pthread_condattr_t *cond_attr);
int pthread_cond_destory(pthread_cond_t cond);
```

#### 3.3.3.2. 操作

```c
int pthread_cond_wait(pthread_cond_t *cond, pthread_mutex_t mutex); 
int pthread_cond_signal(pthread_cond_t cond); 
int pthread_cond_broadcast(pthread_cond_t cond);
```

1. 等待：等到条件变量被通知或广播，等待时会unlock互斥量（原子操作）
	- 当重新开始执行，会lock互斥量
2. 通知：随机唤醒
3. 广播：唤醒所有


