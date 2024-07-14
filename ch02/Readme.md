# 套接字的多种选项
## 套接字可选项和I/O缓冲大小
### 套接字多种可选项
|协议层|选项名|读取|设置|
|:----:|:----:|:----:|:----:|
|SOL_SOCKET|SO_SNDBUF|O|O|
|SOL_SOCKET|SO_RCVBUF|O|O|
|SOL_SOCKET|SO_REUSEADDR|O|O|
|SOL_SOCKET|SO_KEEPALIVE|O|O|
|SOL_SOCKET|SO_BROADCAST|O|O|
|SOL_SOCKET|SO_DONTROUTE|O|O|
|SOL_SOCKET|SO_OOBINLINE|O|O|
|SOL_SOCKET|SO_ERROR|O|O|
|SOL_SOCKET|SO_TYPE|O|O|
|IPPROTO_IP|IP_TOS|O|O|
|IPPROTO_IP|IP_TTL|O|O|
|IPPROTO_IP|IP_MULTICAST_TTL|O|O|
|IPPROTO_IP|IP_MULTICAST_LOOP|O|O|
|IPPROTO_IP|IP_MULTICAST_IF|O|O|
|IPPROTO_TCP|TCP_KEEPALIVE|O|O|
|IPPROTO_TCP|TCP_NODELAY|O|O|
|IPPROTO_TCP|TCP_MAXSEG|O|O|

从表中可以看出，套接字可选项是分层的。
* IPPROTO_IP可选项是IP协议相关事项
* IPPROTO_TCP可选项是TCP协议相关事项
* SOL_SOCKET层是套接字的通用可选项

### `getsockopt` & `setsockopt`
可选项的读取和设置通过以下两个函数来完成
```
#include <sys/socket.h>

int getsockopt(int sock, int level, int optname, void *optval, socklen_t *optlen)
/*
成功时返回0， 失败时返回-1
sock：用于查看选项套接字文件描述符
level：要查看的可选项协议层
optname：要查看的可选名
optval：保存查看结果的缓冲地址值
optlen：向第四个参数传递的缓冲大小。调用函数时，该变量中保存通过第四个参数返回的可选项信息的字节数
*/
```

```
#include <sys/socket.h>

int setsockopt(int sock, int level, int optname, void *optval, socklen_t *optlen)
/*
成功时返回0， 失败时返回-1
sock：用于查看选项套接字文件描述符
level：要查看的可选项协议层
optname：要查看的可选名
optval：保存查看结果的缓冲地址值
optlen：向第四个参数传递的缓冲大小。调用函数时，该变量中保存通过第四个参数返回的可选项信息的字节数
*/
```

### `SO_SNDBUF` & `SO_RCVBUF`
创建套接字的同时会生成I/O缓冲

## `SO_REUSEADDR`
### 发生地址分配错误