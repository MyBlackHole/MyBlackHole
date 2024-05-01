# ss

获取统计socket信息 

## 介绍
ss 命令用来获取统计套接字（Socket）信息，它可以显示与网络相关的统计数据，如连接状态，本地地址，远端地址，进程PID，程序名称等。
## 语法
```
ss [ OPTIONS ] [ FILTER ]
```

## 参数
- OPTIONS：可选参数，用来指定输出内容的详细程度，可选值有：
- -t：显示 TCP 连接信息；
- -u：显示 UDP 连接信息；
- -a：显示所有套接字（TCP/UDP）信息；
- -l：显示本地地址；
- -n：显示 IP 地址，不解析域名；
- -p：显示进程信息；
- -o：显示计时器信息；
- -e：显示详细的套接字信息；
- -m：显示内存使用信息；
- -r：显示路由信息；
- -s：显示 Sockets 摘要信息；
- FILTER：过滤条件，用来指定显示哪些套接字信息，语法为：
- -d：指定协议类型，如：-d tcp、-d udp、-d inet、-d unix；
- -i：指定网络接口，如：-i eth0、-i lo；
- -o：指定本地地址，如：-o 192.168.1.10；
- -p：指定本地端口，如：-p 80；
- -s：指定远端地址，如：-s 192.168.1.1；
- -t：指定套接字状态，如：-t listen、-t established、-t time-wait、-t close-wait；
- -A：显示所有套接字信息；
- -D：显示套接字详细信息；
- -F：显示套接字过滤信息；
- -L：显示套接字本地地址信息；
- -P：显示套接字远端地址信息；
- -S：显示套接字状态信息；
- -T：显示套接字计时器信息；
- -X：显示套接字扩展信息；
- -Z：显示套接字安全信息；

## 安装
- Ubuntu/Debian：
```
sudo apt-get install iputils-ping
sudo apt-get install net-tools
```
- CentOS/RHEL：
```
sudo yum install iputils
sudo yum install net-tools
```
- Arch Linux：
```
sudo pacman -S iputils
sudo pacman -S net-tools
```
- Gentoo：
```
sudo emerge net-tools
```
- macOS：
```
brew install iputils
brew install netstat
```
- Windows：
```
choco install iputils
choco install netstat
```


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
