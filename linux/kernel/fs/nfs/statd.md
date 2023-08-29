RPC.STATD(8)                          System Manager's Manual                       RPC.STATD(8)

NAME
       rpc.statd - NSM服务守护进程

SYNOPSIS
       rpc.statd [-dh?FLNvV] [-H prog] [-n my-name] [-o outgoing-port]
                 [-p listener-port] [-P path]
                 [--nlm-port port] [--nlm-udp-port port]

DESCRIPTION
       文件锁不是持久文件系统状态的一部分。因此当主机重启时锁的状态会丢失。

       当远程主机重启时，网络文件系统必须能够探测到锁状态的丢失。当NFS客户端重启时，NFS服务
       端必须释放该客户端所申请的所有文件锁。在服务端重启后，客户端必须能够提醒服务端它所申
       请的所有文件锁。

       对于NFSv2和NFSv3，使用网络状态监控器Network Status Monitor(NSM)协议来通知NFS对端关于重
       启的事情。在Linux上，NSM服务进程由两个独立的用户空间程序组成：

          ● rpc.statd
              
              该守护进程用于监听其他主机的重启消息，并管理本地主机重启时需要通知的主机列表。

          ● sm-notify
              一个辅助程序，用于在本地系统重启时通知NFS对端。
          
        (译者注：也就是说rpc.statd是重启信息的接收者，而sm-notify是信息的通知者)

       本地NFS锁管理器(NFS lock manager,NLM)会它本地的rpc.statd发出警告说列表中的每个远程对端
       状态都需要被监控。当本地系统重启时，sm-notify命令会通知对端上的NSM服务关于自己重启的
       事。当远程主机重启时，远程对端的sm-notify通知本地的rcp.statd，然后将此信息告诉本地NFS
       锁管理器，本地锁管理器将维护与重启端对应文件的锁。

NSM OPERATION IN DETAIL
       NFS客户端和服务端之间的第一个文件锁交互行为会使得两端的NLM都去联系它们本地的NSM服务以
       存储它们对端的信息。在Linux上，也就是让本地所管理器去联系rpc.statd守护进程。

       rpc.statd会将NFS对端信息记录在持久存储上。该信息描述了如果本地系统重启时如何去联系远程
       对端，如何识别对端报告它在重启的信息，以及当对端说它已经重启完成时如何通知NLM。

       每个客户端在每个文件锁请求中都会发送一个称为客户端caller_name的主机名。NFS服务端可以使
       用该主机名向客户端发送异步GRANT调用，或者通知客户端它已经重启完成。

       Linux NFS服务端可以将客户端的caller_name或客户端的网络地址提供给rpc.statd。为了遵守NSM
       协议，该名称或地址被称为对端mon_name。另外，本地锁管理器会告诉rpc.statd它自己的主机名，
       为了遵守NSM协议，该主机名被称为my_name。

       在NFS中，NFS服务端没有通知客户端它的主机名的交互行为。因此NFS客户端实际上不知道服务端
       的mon_name，尽管在SM_NOTIFY请求中会使用的它。Linux NFS客户端是通过使用从挂载命令中获取
       到的服务端主机名或地址来识别正在启动的NFS服务端的。

   Reboot notification
       当本地系统重启时，本地的sm-notify命令从持久存储中读取监控对端列表，并发送SM_NOTIFY请求
       给列表中每个远程对端的NSM服务进程，它使用mon_name字符串来指定发送目标。为了识别哪个主
       机已经重启完成，重启后的主机会使用sm-notify命令发送my_name字符串。远程rpc.statd将使用
       该字符串与其监控列表中的对端相匹配以找出发送SM_NOTIFY请求的主机。
       
       如果rpc.statd在它的监控列表中未找到能匹配接收到的SM_NOTIFY请求的对端，则通知不会被转发
       给本地锁管理器。另外，每个对端都独有一个32位整数NSM状态码，在每次重启之后，sm-notify命
       令都会让它碰撞重复。rpc.statd使用该号码来区分是重启还是通知重放。

       NFS锁恢复的一部分是重新发现哪个对端需要被再次监控。在每次重启之后，sm-notify命令都会清
       空持久存储上的监控列表。

OPTIONS
       -d, --no-syslog
              若同时指定了"-F"选项，则表示让rpc.statd不再向syslog中写日志，而是输出的stderr中。

       -F, --foreground
              使得rpc.statd附在控制终端上以便NSM操作可以直接被监控到，主要是为了调试。如果不指
              定该选项，则rpc.statd启动后立即进入后台运行。

       -h, -?, --help
              输出rpc.statd命令的使用说明并退出。

       -H, --ha-callout prog
              指定一个高可用callout程序。如果未指定该选项，则不执行任何callout程序。详细说明见
              下面的"High-availability callouts"段落。

       -L, --no-notify
              阻止rpc.statd在其启动时运行sm-notify命令，以及阻止保留已存在的NSM状态号码和监控列
              表。

              注意：sm-notify命令包含了一次检查以确保在每次系统重启后它仅运行了一次。若rpc.statd
              以不带"-L"选项重启时，这可以防止虚假的重启通知。

       -n, --name ipaddr | hostname
              指定RPC监听套接字绑定的地址。如果未指定该选项，rpc.statd使用通配地址(即0.0.0.0)。
              
              该字符串也会被传递给sm-notify命令，用于作为发送重启通知请求时的源地址。详细内容见
              sm-notify(8)的man文档。

       -N     让rpc.statd执行sm-notify命令然后退出。由于sm-notify命令可直接运行，该选项已废弃。

       -o, --outgoing-port port
              指定当sm-notify命令发送重启通知时使用的源端口号。详细内容见sm-notify(8)的man文档。

       -p, --port port
              指定rpc监听套接字监听的端口号。如果未指定该选项，则rpc.statd会尝试从/etc/services中
              获取，如果获取成功，则为所有监听套接字都设置此端口号，否则将为每个监听套接字都选择
              一个随机端口号。
              
              该选项在SM_NOTIFY需要穿越防火墙时可以用来调整端口号，防止被防火墙堵住。

       -T, --nlm-port port
              指定监听NLM请求的端口号。除非使用了"-U"单独指定了UDP端口号，否则将同时监听TCP和UDP的
              端口号。

       -U, --nlm-udp-port port
              指定监听NLM请求的UDP端口号。

       -P, --state-directory-path pathname
              指定NSM状态信息保存路径的父目录。如果未指定该选项，则默认为/var/lib/nfs/statd。

              程序启动之后，rpc.statd将尝试为此目录使用UID和GID设置它的所有者和所属组。

       -v, -V, --version
              输出rpc.statd的版本号并退出。

SECURITY
       The rpc.statd daemon must be started as root to acquire privileges needed to create sockets with
       privileged source ports, and  to  access the state information database.  Because rpc.statd mai-
       ntains a long-running network service, however, it drops root privileges as soon as it starts up
       to reduce the risk of a privilege escalation attack.

       During normal operation, the effective user ID it chooses is the owner of the state directory.  
       This allows  it  to  continue  to  access files in that directory after it has dropped its root
       privileges.  To control which user ID rpc.statd chooses, simply use chown(1) to set the owner of 
       the state directory.

       You can also protect your rpc.statd listeners using the tcp_wrapper library or iptables(8).  To
       use  the  tcp_wrapper  library,  add  the hostnames  of peers that should be allowed access to 
       /etc/hosts.allow. Use the daemon name statd even if the rpc.statd binary has a different filename.

       For further information see the tcpd(8) and hosts_access(5) man pages.

ADDITIONAL NOTES
       主机重启后的锁恢复对于维护数据一致性和防止不必要的应用程序挂起至关重要。为了能让
       rpc.statd更高效地匹配SM_NOTIFY请求，应该遵守一些最佳实践，包括：

              系统的UTS名称需要和NFS对端用来做联系的DNS名称相匹配。
              (注：若不知何为UTS名，可以简单地认为UTS名称就是主机名)

              系统的UTS名称应该总是fqdn格式的名称。

              系统的UTS名的正向和反向DNS映射最好要保持一致。

              客户端用来挂载的服务端的主机名最好能匹配它所发送的SM_NOTIFY请求中的mon_name。

       卸载NFS文件系统时无需停止客户端或服务端任意一端的状态监控。两端可能会继续保持监控一段
       时间，以防这两端间后续再次出现新的挂载和额外的文件锁的NFS流量出现。

       在Linux上，如果没有装在内核锁模块，所有的远程NFS对端都不会被监控。这可能会发生在NFS的
       客户端上，例如，自动挂载工具移除了所有NFS挂载点，因为它们处于空闲非活动状态。

   High-availability callouts
       在rpc.statd成功处理SM_MON, SM_UNMOU和SM_UNMON_ALL请求时或接收到SM_NOTIFY时，rpc.statd可以
       执行一个特定的回调(callout)程序。这个程序在高可用NFS(HA-NFS)环境下可用来跟踪锁状态，特别
       是某节点主机重启后资源需要迁移时。

       callout程序的名称由rpc.statd的"-H"选项指定。该程序运行时有4个参数：第一个参数是add-client
       或del-client或sm-notify，这取决于之所以要调用callout的原因。第二个参数是监控对端的mon_name。
       第三个参数是锁管理器要add-client或del-client时的caller_name，否则则是发送SM_NOTIFY请求的IP
       地址。第四个参数是SM_NOTIFY请求中的state_value。

   IPv6 and TI-RPC support
       TI-RPC is a pre-requisite for supporting NFS on IPv6. If TI-RPC support is built into rpc.statd,
       it attempts to start listeners on network transports  marked 'visible' in /etc/netconfig. As
       long as at least one network transport listener starts successfully, rpc.statd will operate.

FILES
       /var/lib/nfs/statd/sm    directory containing monitor list

       /var/lib/nfs/statd/sm.bak
                                directory containing notify list

       /var/lib/nfs/statd/state NSM state number for this host

       /var/run/run.statd.pid   pid file

       /etc/netconfig           network transport capability database

SEE ALSO
       sm-notify(8),nfs(5),rpc.nfsd(8),rpcbind(8),tcpd(8),hosts_access(5),iptables(8),netconfig(5)

       RFC 1094 - "NFS: Network File System Protocol Specification"
       RFC 1813 - "NFS Version 3 Protocol Specification"
       OpenGroup Protocols for Interworking: XNFS, Version 3W - Chapter 11

AUTHORS
       Jeff Uphoff <juphoff@users.sourceforge.net>
       Olaf Kirch <okir@monad.swb.de>
       H.J. Lu <hjl@gnu.org>
       Lon Hohberger <hohberger@missioncriticallinux.com>
       Paul Clements <paul.clements@steeleye.com>
       Chuck Lever <chuck.lever@oracle.com>

