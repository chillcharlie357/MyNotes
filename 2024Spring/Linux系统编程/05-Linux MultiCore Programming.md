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
title: 05-Linux MultiCore Programming
date:  2024-04-22 11:04
modified:  2024-06-17 22:06
---

# 1. Linux进程

## 1.1. exec

1. exec：直接执行新的程序

```c
#include <unistd.h>

int execl(const char *path, const char *arg0, ..., (char *)0);

int execlp(const char *file, const char *arg0, ..., (char *)0);

int execle(const char *path, const char *arg0, ..., (char *)0, char *const

envp[]);

int execv(const char *path, char *const argv[]);

int execvp(const char *file, char *const argv[]);

int execve(const char *path, char *const argv[], char *const envp[])
```

## 1.2. fork

1. fork：创建一个一样的新进程

```c
#include <sys/types.h>
#include <unistd.h>

pid_t fork(void);
```

使用：

```c
if(fork()==0)
	{子进程执行的代码段；}
else
	{父进程执行的代码段；}
```

## 1.3. 进程退出方式

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

## 1.4. Process resources

每个进程都有一个进程描述符

## 1.5. wait & waitpid

```c
#include <sys/types.h>
#include <sys/wait.h>

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
	2. **非阻塞**
	3. waitpid的pid参数
		1. pid\==-1：对应wait
		2. pid> 0: 指定pid
		3. pid\==0：指定父进程的group
		4. pid<0：指定group id，等待对应组里的进程

## 1.6. signal

进程之间通信

```c
signal.h
```

### 1.6.1. 信号

SIGKILL：终止，不能被捕获或忽略  
SIGINT：终端中断符 , 等价于ctrl + c  
SIGTERM：终止（kill发出的默认系统终止信号），可以改

```c
#include <signal.h>
typedef void (*sighandler_t)(int);
sighandler_t signal(int signum, sighandler_t handler);
//Returned Value: the previous handler if success, SIG_ERR if error
```

### 1.6.2. 可靠性

- 信号可靠性
	1. 连续重复信号能不能收到
		1. 可能会丢失，SIG对应的int值较大对应早期Linux版本不可靠，int值较小对应早期版本;后期版本有可靠机制
	2. 阻塞信号
	3. 复位机制

### 1.6.3. 发信号

- **kill**: send signal to a process

```c
#include <sys/types.h>
#include <signal.h>
int kill(pid_t pid, int sig);
//Returned Value: 0 if success, -1 if failure pid:取值
```

- **raise**: send a signal to the **current process**

```c
#include <signal.h>
int raise(int sig);
//Returned Value: 0 if success, -1 if failure
```

- alarm: set an alarm clock for delivery of a signal
	1. 每个进程只能有一个闹钟
	2. 可以用来做超时处理

```c
#include <unistd.h>
unsigned int alarm(unsigned int seconds);
//Returned value: 0, or the number of seconds remaining of previous alarm 
//SIGALRM
```

- pause: wait for a signal
	1. 挂起，等到有信号来才执行
	2. e.g. CTRL+Z的实现

```c
#include <unistd.h>
int pause(void);
//Returned value: -1, errno is set to be EINTR
```

---

- 例子：

```c
void sig_alrm(int signo){
	printf(“alarm received\n”);
}
unsigned int sleep1(unsigned int nsecs) {
	if ( signal(SIGALARM, sig_alrm) == SIG_ERR)
		return(nsecs);
	alarm(nsecs); /* start the timer */
	pause(); /*next caught signal wakes us up*/
	return(alarm(0) ); /*turn off timer, return unslept time */
}
```

### 1.6.4. 可靠信号

- 信号集

给一个信号注册一个结构体，而不是直接注册处理函数

```c
#include <signal.h>
int sigemptyset(sigset_t *set);
int sigfillset(sigset_t *set);
int sigaddset(sigset_t *set, int signum);
int sigdelset(sigset_t *set, int signum);
//Return value: 0 if success, -1 if error

int sigismember(const sigset_t *set, int signum);
//Return value: 1 if true, 0 if false
```

---

- sigprocmask：检测或更改(或两者)进程的信号掩码

```c
#include <signal.h>
int sigprocmask(int how, const sigset_t *set, sigset_t *oldset);
//Return Value: 0 is success, -1 if failure
```

- 参数“**how**”决定对信号掩码的操作
	1. SIG_BLOCK: 将set中的信号**添加**到信号掩码(并集)
	2. SIG_UNBLOCK: 从信号掩码中**去掉**set中的信号(差集)
	3. SIG_SETMASK: 把信号掩码**设置为**set中的信号
- 在sigprocmask调用后任何未阻塞并且pending的信号，在函数返回前，至少有一个信号会送达进程
- 例外: SIGKILL, SIGSTOP

---

- sigpending: 返回当前未决的信号集

```c
#include <signal.h>
int sigpending(sigset_t *set);
//Returned Value: 0 is success, -1 if failure
```

---

- **sigaction**：检查或修改与指定信号的关联处理动作

```c
#include <signal.h>
int sigaction(int signum, const struct sigaction *act, struct sigaction *oldact);
//Returned Value: 0 is success, -1 if failure)
```

- struct sigaction成员：

```c
handler_t sa_handler; /* addr of signal handler, or SIG_IGN, 
or SIG_DEL */
sigset_t sa_mask; /* additional signals to block */
int sa_flags; /* signal options */
```

---

- `sigsuspend`：使用临时信号替代信号掩码，在捕获一个信号或发生终止该进程的信号前，进程挂起

```c
#include <signal.h>
int sigsuspend(const sigset *sigmask);
//Returned value: -1, errno is set to be EINTR
```


## 1.7. 可重入函数

- 可重入：可以被打断的函数
- 不可重入函数：
	1. 系统资源
	2. 全局变量
	3. 使用静态数据结构
	4. 调用malloc或者free
	5. 标准IO函数

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

- flags
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

- 和普通thread区别
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

```shell
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
	void *arg)；
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

- sem_init参数
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

 等待某个变量满足条件，和**互斥量一起使用**

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

1. 等待：等到条件变量被通知或广播，**等待时会unlock互斥量（原子操作）**
	- 当重新开始执行，会lock互斥量
2. 通知：随机唤醒
3. 广播：唤醒所有

#### 3.3.3.3. 例子

加数据，拿数据并发修改index，len

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2024%2F05%2F13%2F10-14-29-585bba0567fdf55505009b82a7129584-20240513101428-8e6943.png)

---

```c
pthread_mutex_t mutex;
pthread_cond_t condition;
int count = 0;

void decrement_count() {
    pthread_mutex_lock(&mutex);
    while (count == 0)
        pthread_cond_wait(&condition, &mutex);
    count = count - 1;
    pthread_mutex_unlock(&mutex);
}

void increment_count() {
    pthread_mutex_lock(&mutex);
    if (count == 0)
        pthread_cond_signal(&condition);
    count = count + 1;
    pthread_mutex_unlock(&mutex);
}
```

`pthread_cond_wait`之后的代码仍然在互斥区里，不用担心等待被唤醒后有并发问题。

## 3.4. Thread attributes

- 线程属性对象
- 初始化：`int pthread_attr_init(pthread_attr * attr);`
- get/set族函数，获取/设置属性

e.g.修改detachstate，schedpolicy属性

## 3.5. Thread cancellation

线程强制终止。

```c
int pthread_cancel(pthread_t thread);
int pthread_setcancelstate(int state, int 
*oldstate);
int pthread_setcanceltype(int type, int *oldtype);
```

## 3.6. Multithread program

- 容易出现错误
	1. 共享变量缺乏保护，未互斥使用
	2. 特别的，创建线程时传递指针，指针指向的变量可能时共享的

系统不隔离，用起来方便，但不安全

## 3.7. Thread Local Storage (TLS)

线程局部存储：变量是**线程私有**的，对于线程内部的函数是全局变量

函数内的局部变量在线程的函数调用栈里，本来就不存在全局共享

```c
int pthread_key_create(pthread_key_t *key, void (*destructor)(void*));

int pthread_key_delete(pthread_key_t key);

void *pthread_getspecific(pthread_key_t key);

int pthread_setspecific(pthread_key_t key, const void *value);
```

1. pthread_key_create：key相当于变量名，每一个线程都创建了这个变量，只是隔离了
	- 在每个线程中**同时创建，同时释放**
2. delete：会调用create时传入的析构函数                                                  
3. get/set：对TLS读写操作

