# ss
获取统计socket信息 

## 参数
### -h 
显示帮助信息； 
### -V 
显示指令版本信息； 
### -n 
不解析服务名称，以数字方式显示； 
### -a 
显示所有的套接字； 
### -l 
显示处于监听状态的套接字； 
### -o 
显示计时器信息； 
### -m 
显示套接字的内存使用情况； 
### -p 
显示使用套接字的进程信息； 
### -i 
显示内部的TCP信息； 
### -4 
只显示ipv4的套接字； 
### -6 
只显示ipv6的套接字； 
### -t 
只显示tcp套接字； 
### -u 
只显示udp套接字； 
### -d 
只显示DCCP套接字； 
### -w 
仅显示RAW套接字； 
### -x 
仅显示UNIX域套接字。 



## 例子
- ss -ta 
显示ICP连接 

- ss –s 
显示Sockets摘要 

- ss –l 
列出所有打开的网络连接端口 

- ss –pl 
查看进程使用的socket 

- ss -ua  
显示所有UDP Sockets 

-  显示本地监听的端口 
```shell
# 代码位置 net/ipv4/tcp_diag.c
ss -tnlp
ss –lnt
ss -ln4
udp   UNCONN     0      0                                     *:2049                                              *:*
udp   UNCONN     0      0                                     *:52109                                             *:*
udp   UNCONN     0      0                                     *:20048                                             *:*
udp   UNCONN     0      0                                     *:111                                               *:*
udp   UNCONN     0      0                                     *:123                                               *:*
udp   UNCONN     0      0                             127.0.0.1:323                                               *:*
udp   UNCONN     0      0                                     *:41614                                             *:*
udp   UNCONN     0      0                                     *:954                                               *:*
udp   UNCONN     0      0                             127.0.0.1:1001                                              *:*
tcp   LISTEN     0      511                                   *:8008                                              *:*
tcp   LISTEN     0      100                                   *:8009                                              *:*
tcp   LISTEN     0      128                                   *:48457                                             *:*
tcp   LISTEN     0      1024                                  *:3306                                              *:*
tcp   LISTEN     101    100                           127.0.0.1:41387                                             *:*
tcp   LISTEN     0      1024                                  *:6379                                              *:*
tcp   LISTEN     0      64                                    *:41035                                             *:*
tcp   LISTEN     0      128                                   *:111                                               *:*
tcp   LISTEN     0      1024                                  *:8080                                              *:*
tcp   LISTEN     0      128                                   *:20048                                             *:*
tcp   LISTEN     0      128                                   *:22                                                *:*
tcp   LISTEN     0      128                           127.0.0.1:8889                                              *:*
tcp   LISTEN     0      1024                                  *:8793                                              *:*
tcp   LISTEN     0      100                           127.0.0.1:25                                                *:*
tcp   LISTEN     0      64                                    *:2049                                              *:*

对于 LISTEN 状态的 socket
Recv-Q：当前全连接队列的大小，即已完成三次握手等待应用程序 accept() 的 TCP 链接
Send-Q：全连接队列的最大长度，即全连接队列的大小 


对于非 LISTEN 状态的 socket
Recv-Q：已收到但未被应用程序读取的字节数
Send-Q：已发送但未收到确认的字节数
```

- sudo ss -tlnp
