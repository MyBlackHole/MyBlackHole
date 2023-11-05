# config

## net.ipv4.tcp_tw_recycle
4 版本不支持

1: 表示开启TCP连接中TIME-WAIT sockets的快速回收
0: 表示关闭 (默认)


## net.ipv4.tcp_tw_reuse
1: 表示开启重用。允许将TIME-WAIT sockets重新用于新的TCP连接
0: 表示关闭(默认)


```shell
net.ipv4.tcp_syncookies=1 打开TIME-WAIT套接字重用功能，对于存在大量连接的Web服务器非常有效。 
net.ipv4.tcp_tw_recyle=1 
net.ipv4.tcp_tw_reuse=1 减少处于FIN-WAIT-2连接状态的时间，使系统可以处理更多的连接。 
net.ipv4.tcp_fin_timeout=30 减少TCP KeepAlive连接侦测的时间，使系统可以处理更多的连接。 
net.ipv4.tcp_keepalive_time=1800 增加TCP SYN队列长度，使系统可以处理更多的并发连接。 
net.ipv4.tcp_max_syn_backlog=8192

net.ipv4.tcp_syncookies = 1
#表示开启SYN Cookies。当出现SYN等待队列溢出时，启用cookies来处理，可防范少量SYN攻击，默认为0，表示关闭；
net.ipv4.tcp_tw_reuse = 1
#表示开启重用。允许将TIME-WAIT sockets重新用于新的TCP连接，默认为0，表示关闭；
net.ipv4.tcp_tw_recycle = 1
#表示开启TCP连接中TIME-WAIT sockets的快速回收，默认为0，表示关闭。
net.ipv4.tcp_fin_timeout = 30
#表示如果套接字由本端要求关闭，这个参数决定了它保持在FIN-WAIT-2状态的时间。
net.ipv4.tcp_keepalive_time = 1200 
#表示当keepalive起用的时候，TCP发送keepalive消息的频度。缺省是2小时，改为20分钟。
net.ipv4.ip_local_port_range = 1024    65000 
#表示用于向外连接的端口范围。缺省情况下很小：32768到61000，改为1024到65000。
net.ipv4.tcp_max_tw_buckets = 5000
#表示系统同时保持TIME_WAIT套接字的最大数量，如果超过这个数字，
#TIME_WAIT套接字将立刻被清除并打印警告信息。默认为180000，改为5000。
#对于Apache、Nginx等服务器，上几行的参数可以很好地减少TIME_WAIT套接字数量，
#但是对于Squid，效果却不大。此项参数可以控制TIME_WAIT套接字的最大数量，避免Squid服务器被大量的TIME_WAIT套接字拖死
```
