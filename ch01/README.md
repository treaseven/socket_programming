# 理解网络编程和套接字
### 重要函数
生成套接字
```
#include <sys/socket.h>
int socket(int domain, int type, int protocol);
```
绑定地址
```
#include <sys/socket.h>
int bind(int socket, const struct sockaddr *address, socklen_t address_len);
```
监听连接
```
#include <sys/socket.h>
int listen(int socket, int backlog);
```
接受请求
```
#include <sys/socket.h>
int accept(int sockfd, const struct sockaddr *addr, socklen_t *addrlen);
```
请求连接
```
#include <sys/socket.h>
int connect(int socket, const struct sockaddr *address, socklen_t address_len);
```

### Linux文件操作
文件描述符（文件句柄）：是系统分配给文件或套接字的整数
系统文件描述符
|文件描述符|对象|
|:----:|:----:|
|0|标准输入：Standard Input|
|1|标准输出：Standard Output|
|2|标准错误：Standard Error|
#### I/O函数
打开文件
```
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
int open(const char *path, int flag);
```
文件打开模式(flag):
|打开模式|含义|
|:----:|:----:|
|O_CREAT|必要时创建文件|
|O_TRUNC|删除全部现有数据|
|O_APPEND|追加到已有数据后面|
|O_RDONLY|只读打开|
|O_WRONLY|只写打开|
|O_RDWR|读写打开|

关闭文件
```
#inlcude <unistd.h>
int close(int fd);
```
写文件
```
#include <unistd.h>
ssize_t write(int fd, const void *buf, size_t nbytes);
```
