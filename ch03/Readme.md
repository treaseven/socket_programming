# 多进程服务器端
## 进程概念及应用
### 并发服务端的实现方法
* 多进程服务器：通过创建多个进程提供服务
* 多路复用服务器：通过捆绑并统一管理I/O对象提供服务
* 多线程服务器：通过生成与客户端等量的线程提供服务
### 通过调用fork函数创建进程
```
#include <unistd.h>
pid_t fork(void)
```
fork函数的如下特点区分程序执行流程
* 父进程：fork函数返回子进程ID
* 子进程：fork函数返回0

## 进程和僵尸进程
### 僵尸进程
进程的工作完成后应被销毁，但有时这些进程将变成僵尸进程，占用系统中的重要资源，这种状态下的进程称作僵尸进程
### 产生僵尸进程的原因
### 销毁僵尸进程1：利用wait函数
为了销毁子进程，父进程应该主动请求获取子进程的返回值
```
#include <sys/wait.h>
pid_t wait(int *statloc);
/*
成功时返回终止的子进程ID，失败时返回-1
*/
```
### 销毁僵尸进程2：使用waitpid函数
wait函数会引起程序阻塞，还可以考虑调用waitpid函数，这是防止僵尸进程的第二种方法，也是防止阻塞的方法
```
#include <sys/wait.h>
pid_t waitpid(pid_t pid, int *statloc, int options);
/*
成功时返回终止的子进程ID或0，失败时返回-1
pid：等待终止的目标子进程的ID，若传-1，则与wait函数相同，可以等待任意子进程终止
statloc：与wait函数的statloc参数具有相同含义
options：传递头文件sys/wait.h声明的变量WNOHANG，即使没有终止的子进程也不会进入阻塞状态，而是返回0退出函数
*/
```
## 信号处理
### 信号与signal函数
```
#include <signal.h>
void (*signal(int signo, void (*func)(int)))(int);
/*
为了在产生信号时调用，返回之前注册的函数指针
函数名：signal
参数：int signo，void (*func)(int)
返回类型：参数类型为int型，返回void型函数指针
*/
```
### 利用sigaction函数进行信号处理
```
#include <signal.h>
int sigaction(int signo, const struct sigaction *act, struct sigaction *oldact);
/*
成功时返回0，失败时返回-1
act：对于第一个参数的信号处理函数信息
oldact：通过此参数获取之前注册的信号处理函数指针，若不需要则传递0
*/
```
声明并初始化sigaction结构体变量以调用上述函数，该结构体定义如下：
```struct sigaction
{
    void (*sa_handler)(int);
    sigset_t sa_mask;
    int sa_flags;
}
```
