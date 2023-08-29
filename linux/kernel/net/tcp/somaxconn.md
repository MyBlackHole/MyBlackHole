# somaxconn
TCP 全连接队列的最大长度由 min(somaxconn, backlog) 控制，其中：

somaxconn 是 Linux 内核参数，由 /proc/sys/net/core/somaxconn 指定
backlog 是 TCP 协议中 listen 函数的参数之一，即 int listen(int sockfd, int backlog) 函数中的 backlog 大小
在 Golang 中，listen 的 backlog 参数使用的是 /proc/sys/net/core/somaxconn 文件中的值。

代码位置：net/socket.c:__sys_listen
