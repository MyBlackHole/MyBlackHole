# socket

net/socket.c:socket
    net/socket.c:__sys_socket
        net/socket.c:__sys_socket_create
            net/socket.c:sock_create
                net/socket.c:__sock_create
                    net/socket.c:sock_alloc

![[imgs/socket.png]]


# TCP
net/socket.c:recv
    net/socket.c:__sys_recvfrom
        net/socket.c:sock_recvmsg
            net/socket.c:sock_recvmsg_nosec
                net/socket.c:sock->ops->recvmsg(net/core/sock.c:sock_common_recvmsg)
                    net/core/sock.c:sk->sk_prot->recvmsg
                        net/ipv4/tcp_ipv4.c:tcp_prot.recvmsg
                            net/ipv4/tcp.c:tcp_recvmsg
                                net/ipv4/tcp.c:tcp_recvmsg_locked
                                    net/ipv4/tcp.c:sk_wait_data
                                        include/net/sock.h:sched_annotate_sleep
                                            kernel/sched/wait.c:wait_woken
                                                kernel/sched/timer.c:schedule_timeout
                                                    kernel/sched/core.c:schedule
                                                        kernel/sched/core.c:__schedule
                                                            kernel/sched/core.c:deactivate_task
                                                            kernel/sched/core.c:pick_next_task
![[imgs/socket-1.png]]
![[imgs/socket-2.png]]

# listen

![[imgs/socket-3.png]]
[细节](https://heapdump.cn/article/2300883)
