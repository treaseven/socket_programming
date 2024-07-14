# 多线程服务器端的实现
## 理解线程的概念
线程比进程具有的优点：
* 线程的创建和上下文切换比进程的创建和上下文切换更快
* 线程间交换数据无需特殊技术
### 线程和进程的差异
线程是为了解决：为了得到多条代码执行流而复制整个内存区域的负担太重
## 线程创建及运行
### 线程的创建和执行流程
```
#include <pthread.h>
int pthread_create(pthread_t *restrict thread, 
                   const pthread_attr_t *restrict attr, 
                   void *(*start_routine)(void *), 
                   void *restrict arg);
/*
成功时返回0，失败时返回-1
thread：保存新创建线程ID的变量地址值，线程与进程相同，也需要区分不同线程的ID
attr：用于传递线程属性的参数，传递NULL时，创建默认属性的线程
start_routine：相当于线程main函数的、在单独执行流中执行的函数地址值(函数指针)
arg：通过第三个参数传递的调用函数时包含传递参数信息的变量地址值
```
```
#include <pthread.h>
int pthread_join(pthread_t thread, void **status);
/*
成功时返回0，失败时返回-1
thread：该参数值ID的线程终止后才会从该函数返回
status：保存线程的main函数返回值的指针的变量地址值
*/
```
### 可在临界区调用的函数
临界区块指的是一个访问共享资源的程序片段，而这些共享资源有无法同时被多个线程访问的特性
## 线程同步
### 同步的两面性
线程同步用于解决线程访问顺序引发的问题
* 同时访问同一内存空间时发生的情况
* 需要指定访问同一内存空间的线程顺序的情况
### 互斥量
互斥锁是一种用于多线成编址中，防止两条线程同时对同一公共资源进行读写的机制。该目的通过将代码切片成一个个的临界区域。临界区域指的是一块对公共资源进行访问的代码，并非一种机制或是算法。
```
#include <pthread.h>
int pthread_mutex_init (pthread_mutex_t *mutex, const pthread_mutexattr_t *attr);
int pthread_mutex_destroy(pthread_mutex_t *mutex);
/*
成功时返回0，失败时返回其他值
mutex：创建互斥量时传递保存互斥量的变量地址值，销毁时传递需要销毁的互斥量地址
attr：传递即将创建的互斥量属性，没有特别需要指定的属性时传递NULL
*/
```
```
#include <pthread.h>
int pthread_mutex_lock(pthread_mutex_t *mutex);
int pthread_mutex_unlock(pthread_mutex_t *mutex);
```
### 信号量
信号量又称为信号标，是一个同步对象，用于保持在0至指定最大值之间的一个计数值
```
#include <semaphore.h>
int sem_init(sem_t *sem, int pshared, unsigned int value);
int sem_destroy(sem_t *sem);
/*
成功时返回0， 失败时返回其他值
sem：创建信号量时保存信号量的变量地址值，销毁时传递需要销毁的信号量变量地址值
pshared：传递其他值，创建可由多个继承共享的信号量；传递0时，创建只允许1个进程内部的信号量。需要完成同一进程的线程同步，故为0
value：指定创建信号量的初始值
*/
```
#include <semaphore.h>
int sem_post(sem_t *sem);
int sem_wait(sem_t *sem);
```
## 线程的销毁和多线程并发服务器端的实现
### 销毁线程的3种方法
* 调用pthread_join函数
* 调用pthread_detach函数