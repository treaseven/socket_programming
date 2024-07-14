# 优于select的epoll
## epoll理解及应用
### 基于select的I/O复用技术速度慢的原因
select性能上最大的弱点：每次传递监视对象信息
### 实现epoll时必要的函数和结构体
* 无需编写以监视状态变化为目的针对所有文件描述符的循环语句
* 调用对应于select函数的epoll_wait函数时无需每次传递监视对象信息
epoll函数的功能
* epoll_create：创建保存epoll文件描述符的空间
* epoll_ctl：向空间注册并注销文件描述符
* epoll_wait：与select函数类似，等待文件描述符发生变化
```
struct epoll_event
{
    __uint32_t events;
    epoll_data_t data;
};
typedef union epoll_data {
    void *ptr;
    int fd;
    __uint32_t u32;
    __uint64_t u64;
} epoll_data_t;
```
epoll_events的成员events中可以保存的常量及所指的事件类型
* EPOLLIN：需要读书的情况
* EPOLLOUT：输出缓冲为空，可以立即发送数据的情况
* EPOLLPRI：收到OOB数据的情况
* EPOLLRDHUP：断开连接或半关闭的情况，这在边缘触发方式下非常有用
* EPOLLERR：发生错误的情况
* EPOLLET：以边缘触发的方式得到事件通知
* EPOLLONESHOT：发生一次事件后，相应文件描述符不再收到事件通知
### `epoll_create`
```
#include <sys/epoll.h>
int epoll_create(int size);
/*
成功时返回epoll的文件描述符，失败时返回-1
size: epoll实例的大小
*/
```
### `epoll_ctl`
生成例程，应在其内部注册监视对象文件描述符
```
#include <sys/epoll.h>
int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event);
/*
成功时返回0，失败时返回-1
epfd：用于注册监视对象的epoll例程的文件描述符
op：用于指定监视对象的添加、删除或更改等操作
fd：需要注册的监视对象文件描述符
event：监视对象的事件类型
*/
```
### `epoll_wait`
```
#include <sys/epoll.h>
int epoll_wait(int epfd, struct epoll_event *events, int maxevents, int timeout);
/*
成功时返回发生事件的文件描述符个数，失败时返回-1
epfd：表示事件发生监视范围的epoll例程的文件描述符
events：保存发生事件的文件描述符集合的结构体地址值
maxevents：第二个参数中可以保存的最大事件数
timeout：以1/1000秒为单位的等待事件，传递-1时，一直等待直到发生事件
*/
```
总结一下epoll的流程：
1.epoll_create创建一个保存epoll文件描述符的空间，可以没有参数
2.动态分配内存，给将要监视的epoll_wait
3.利用epoll_ctl控制、添加、删除，监听事件
4.利用epoll_wait来获取改变的文件描述符，来执行程序