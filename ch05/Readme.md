```
#include <sys/select.h>
#include <sys/time.h>

int select(int maxfd, fd_set *readset, fd_set *writeset, fd_set *exceptset, const struct timeval *timeout);
/*
成功时返回大于0的值，失败时返回-1
maxfd：监视对象文件描述数量
readset: 将所有关注是否存在待读取数据的文件描述符注册到fd_set型变量，并传递其地址值
writeset: 将所有关注是否传输无阻塞数据的文件描述符注册到fd_set型变量，并传递其地址值
exceptset: 将所有关注是否发生异常的文件描述符注册到fd_set型变量，并传递其地址值
timeout：调用select函数后，为防止陷入无限阻塞的状态，传递超时信息
返回值：发生错误时返回-1，超时返回0.
*/
```