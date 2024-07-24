# rpcbind
![[imgs/rpcbind.png]]

配置 rpcbind 监听方式: /etc/netconfig

## start 
```shell
# 在有连接来时，自动启动 rpcbind.service
sudo systemctl status rpcbind.socket

sudo lsof -i:111
COMMAND PID USER   FD   TYPE  DEVICE SIZE/OFF NODE NAME
systemd   1 root   99u  IPv4 7321346      0t0  TCP *:sunrpc (LISTEN)
systemd   1 root  101u  IPv4 7325335      0t0  UDP *:sunrpc
systemd   1 root  102u  IPv6 7318063      0t0  TCP *:sunrpc (LISTEN)
systemd   1 root  104u  IPv6 7327004      0t0  UDP *:sunrpc


sudo systemctl status rpcbind.service
○ rpcbind.service - RPC bind portmap service
     Loaded: loaded (/lib/systemd/system/rpcbind.service; disabled; preset: enabled)
     Active: inactive (dead)
TriggeredBy: ● rpcbind.socket
       Docs: man:rpcbind(8)


rpcinfo
   program version netid     address                service    owner
    100000    4    tcp6      ::.0.111               portmapper superuser
    100000    3    tcp6      ::.0.111               portmapper superuser
    100000    4    udp6      ::.0.111               portmapper superuser
    100000    3    udp6      ::.0.111               portmapper superuser
    100000    4    tcp       0.0.0.0.0.111          portmapper superuser
    100000    3    tcp       0.0.0.0.0.111          portmapper superuser
    100000    2    tcp       0.0.0.0.0.111          portmapper superuser
    100000    4    udp       0.0.0.0.0.111          portmapper superuser
    100000    3    udp       0.0.0.0.0.111          portmapper superuser
    100000    2    udp       0.0.0.0.0.111          portmapper superuser
    100000    4    local     /run/rpcbind.sock      portmapper superuser
    100000    3    local     /run/rpcbind.sock      portmapper superuser


sudo systemctl status rpcbind.service
● rpcbind.service - RPC bind portmap service
     Loaded: loaded (/lib/systemd/system/rpcbind.service; disabled; preset: enabled)
     Active: active (running) since Sat 2023-06-10 16:34:49 CST; 2s ago
TriggeredBy: ● rpcbind.socket
       Docs: man:rpcbind(8)
   Main PID: 1500069 (rpcbind)
      Tasks: 1 (limit: 18298)
     Memory: 660.0K
        CPU: 3ms
     CGroup: /system.slice/rpcbind.service
             └─1500069 /sbin/rpcbind -f -w

6月 10 16:34:49 black systemd[1]: Starting rpcbind.service - RPC bind portmap service...
6月 10 16:34:49 black systemd[1]: Started rpcbind.service - RPC bind portmap service.



sudo ./rpcbind
sudo lsof -i:111
COMMAND     PID USER   FD   TYPE  DEVICE SIZE/OFF NODE NAME
rpcbind 1857005 root    6u  IPv4 8743850      0t0  UDP *:sunrpc
rpcbind 1857005 root    7u  IPv4 8743851      0t0  TCP *:sunrpc (LISTEN)
rpcbind 1857005 root    8u  IPv6 8743852      0t0  UDP *:sunrpc
rpcbind 1857005 root    9u  IPv6 8743853      0t0  TCP *:sunrpc (LISTEN)
```

## 参数
```shell
-f : 前台启动
-w : 热启动
-d : debug
-a : 在调试模式下，遇到错误时强制中断。
-d : 运行调试模式。在此模式下，rpcbind将记录额外的信息，如果指定了"-a"选项，则出现某些
   : 错误时中断。使用该选项，name-to-address转换的一致性会一直被检测且详细记录相关信息。
-f : 不进行fork且运行在后台。
-h : 指定绑定到UDP请求的特定IP地址。该选项可多次指定，在多宿主主机(multi-homed即具有多
   : 个网络接口的主机)该选项通常是必须的。如果不给定"-h"选项，rpcbind将绑定到INADDR_ANY
   : 上，这将导致当多宿主主机上的rpcbind从非UDP数据包接收的地址返回UDP响应包时出错。注
   : 意，当使用"-h"指定IP地址时，rpcbind将自动向列表中添加127.0.0.1，如果启用IPv6则还添
   : 加::1。
-i : 以"不安全"(Insecure)模式运行。运行从任何主机上调用SET和UNSET。一般情况下，出于安全，
   : rpcbind仅从环回接口上接收请求。若要兼容早期不使用环回接口接收请求的版本，该选项是
   : 必须的。
-l : 启用libwrap连接日志。
-s : 立即改变rpcbind用户daemon相关权限。这会导致rpcbind使用非特权端口流出数据，阻止非特权
   : 客户端使用rpcbind连接服务器上特权端口对应的服务。
-w : 在rpcbind启动时，让rpcbind读取state文件做温启动(warm start)，state文件是rpcbind上次终
   : 止时创建的。
   : (译者注：冷启动(cold start)是指完全以初始化的状态启动进程，温启动是指以某种方式恢复
   : 进程的状态。所以冷启动的进程不会继承任何原进程的数据和状态。)

如果rpcbind重启了，所有RPC管理的服务都必须重启。
```
