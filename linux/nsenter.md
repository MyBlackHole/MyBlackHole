# nsenter

是一个可以在指定进程的命令空间下运行指定程序的命令。它位于util-linux包中

一个最典型的用途就是进入容器的网络命令空间
相当多的容器为了轻量级，是不包含较为基础的命令的
比如说 ip address，ping，telnet，ss，tcpdump 等等命令
这就给调试容器网络带来相当大的困扰：只能通过 docker inspect ContainerID 命令获取到容器IP，以及无法测试和其他网络的连通性
这时就可以使用nsenter命令仅进入该容器的网络命名空间，使用宿主机的命令调试容器网络
此外，nsenter也可以进入 mnt, uts, ipc, pid, user 命令空间，以及指定根目录和工作目录

## namespace
namespace是Linux中一些进程的属性的作用域，使用命名空间，可以隔离不同的进程。

Linux在不断的添加命名空间，目前有：

mount：挂载命名空间，使进程有一个独立的挂载文件系统，始于Linux 2.4.19
ipc：ipc命名空间，使进程有一个独立的ipc，包括消息队列，共享内存和信号量，始于Linux 2.6.19
uts：uts命名空间，使进程有一个独立的hostname和domainname，始于Linux 2.6.19
net：network命令空间，使进程有一个独立的网络栈，始于Linux 2.6.24
pid：pid命名空间，使进程有一个独立的pid空间，始于Linux 2.6.24
user：user命名空间，是进程有一个独立的user空间，始于Linux 2.6.23，结束于Linux 3.8
cgroup：cgroup命名空间，使进程有一个独立的cgroup控制组，始于Linux 4.6
Linux的每个进程都具有命名空间，可以在/proc/PID/ns目录中看到命名空间的文件描述符。

## 用法
nsenter [选项] [<程序> [<参数>...]]

以其他程序的名字空间运行某个程序。

选项：
 -a, --all              enter all namespaces
 -t, --target <pid>     要获取名字空间的目标进程
 -m, --mount[=<文件>]   进入 mount 名字空间
 -u, --uts[=<文件>]     进入 UTS 名字空间(主机名等)
 -i, --ipc[=<文件>]     进入 System V IPC 名字空间
 -n, --net[=<文件>]     进入网络名字空间
 -p, --pid[=<文件>]     进入 pid 名字空间
 -C, --cgroup[=<文件>]  进入 cgroup 名字空间
 -U, --user[=<文件>]    进入用户名字空间
 -T, --time[=<file>]    enter time namespace
 -S, --setuid <uid>     设置进入空间中的 uid
 -G, --setgid <gid>     设置进入名字空间中的 gid
     --preserve-credentials 不干涉 uid 或 gid
 -r, --root[=<目录>]     设置根目录
 -w, --wd[=<dir>]       设置工作目录
 -F, --no-fork          执行 <程序> 前不 fork
 -Z, --follow-context  根据 --target PID 设置 SELinux 环境

 -h, --help             display this help
 -V, --version          display version

如果没有给出program，则默认执行$SHELL。

## 例子

```shell
docker inspect -f {{.State.Pid}} 0e62c9eb9752
27991

sudo nsenter -n -t27991
root@black:/home/black# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
20: eth0@if21: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
root@black:/home/black#
```
