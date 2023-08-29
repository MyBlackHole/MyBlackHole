rpc.mountd(8)                  System Manager's Manual                              rpc.mountd(8)

NAME
       rpc.mountd - NFS mount守护进程

SYNOPSIS
       /usr/sbin/rpc.mountd [options]

DESCRIPTION
       rpc.mountd守护进程用于实现NFS服务端的NFS MOUNT协议。

       NFS服务端会维护一张可被NFS客户端访问的本地物理文件系统的表。表中的每个文件系统都被称为
       导出的文件系统，或简称为导出项。

       导出表中的每个文件系统都有单独的访问控制列表(acl)。rpc.mountd使用这些acl来决定NFS客户端
       是否允许访问对应的文件系统。关于如何管理NFS服务端的导出表，参见export(5)和exportfs(8)的
       man文档。

   Mounting exported NFS File Systems
       NFS MOUNT协议有几个过程组成。最主要的是MNT(mount an export)和UMNT(unmount an export)。
       
       MNT请求有两个参数：一个显式参数，它包含待挂载的导出项的根目录的路径，还有一个隐式参数，
       表示的是MNT请求发送者的IP地址。

       当NFS客户端发送的MNT请求被接收时，rpc.mountd根据导出表检查路径名和发送者的IP地址，如果
       发送者被允许访问其所请求的导出项，rpc.mountd将为导出根目录返回一个NFS文件句柄给客户端。
       然后客户端就可以使用该根文件句柄，NFS的文件查找请求也可以在导出项的目录结构中穿行。

   The rmtab File
       每当MNT请求成功(即成功挂载导出项)，rpc.mountd都会为向/var/lib/nfs/rmtab文件中写一条信息。
       当接收到来自NFS客户端的UMNT请求时，rpc.mountd会简单地从/var/lib/nfs/rmtab文件中移除对应
       对应项。

       客户端可以使用showmount(8)命令探测NFS服务端已导出的文件系统列表，或列出服务端被其它客户
       端挂载的导出文件系统。showmount(8)使用的是NFS MOUNT协议中另一个程序，它用来输出服务端已
       导出的文件系统的关于信息。

       但注意，/var/lib/nfs/rmtab文件的内容并不一定准确无误。即使在调用UMNT后，客户端仍可能继续
       访问导出项。如果客户端未发送UMNT请求就重启了，/var/lib/nfs/rmtab中对应项仍会保留，但已经
       过期无效。

OPTIONS
       -d kind  or  --debug kind
              开启调试功能。有效的kind值为：all, auth, call, general和parse。

       -F  or  --foreground
              使rpc.mountd运行在前台。

       -f export-file  or  --exports-file export-file
              该选项指定exports文件，并列出其中的所有客户端对象以及对应的mount选项(见exports(5))。
              默认的exports文件为/etc/exports。

       -h  or  --help
              输出命令使用方法。

       -o num  or  --descriptors num
              设置能打开的文件描述符最大数量。

       -N mountd-version  or  --no-nfs-version mountd-version
              该选项可让rpc.mount不向特定版本的NFS提供服务。

       -n  or  --no-tcp
              不要为mount发送tcp广播。

       -p num  or  -P num  or  --port num
              指定用于RPC监听套接字的端口号。如果未指定该选项，rpc.mountd将尝试从/etc/services中获
              取，如果获取成功，则所有监听套接字设置都设置为此端口，否则为每个监听套接字选择一个
              随机临时的端口

              当NFS MOUNT请求必须穿过防火墙时，可以使用该选项调整rpc.mountd的监听端口。

       -H  prog or  --ha-callout prog
              指定一个高可用的唤起程序(callout program)。该程序会接收所有MOUNT和UNMOUNT请求的唤起。
              这使得rpc.mountd可被用于高可用NFS(HA-NFS)环境。

              该唤起程序使用4个参数来运行。第一个参数是mount或unmount唤起的原因(The first is mount 
              or unmount depending on the reason for the callout)。第二个参数是正在运行mount的客户端
              名称。第三个参数是客户端正在挂载的导出路径。第四个参数是客户端具有该路径的并发mount
              数(The last is the umber of concurrent mounts that we believe the client has of that path
              )。

              在kernel 2.6和之后的版本中不需要指定唤起程序。取而代之的是，将NFS文件系统挂载在
              /proc/fs/nfsd上。

       -s, --state-directory-path directory
              指定放置rpc.statd状态信息的目录。如果未指定该选项，则默认路径为/var/lib/nfs。

       -r, --reverse-lookup
              rpc.mountd会跟踪rmtab文件中IP地址。当发起了一个DUMP请求(例如某用户运行了showmount -a)
              时，默认将返回IP地址而不是主机名。该选项使得rpc.mountd反向解析IP地址为主机名并返回。
              启用该选项在某些环境下会大幅度降低性能。
              
       -t N or --num-threads=N or --num-threads N
              该选项指定rpc.mountd spawn出的线程数。默认为1个线程，一般已经足够了。只有在NFS几秒内
              需要处理成百上千个NFS mount请求风暴时，或DNS解析服务很慢甚至不可到达时，才可能需要使
              用更多的线程。

       -u  or  --no-udp
              禁止为mount发送UDP广播。

       -V version  or  --nfs-version version
              指定rpc.mountd可以为特定版本的NFS提供服务。当前版本的rpc.mountd支持NFSv2以上的所有版
              本。

       -v  or  --version
              输出rpc.mountd的版本信息并退出。

       -g  or  --manage-gids
              接收来自内核的请求，为使用访问控制列表，将uid映射到gid列表中。一般情况下，NFS请求中
              包含了一个UID和GID列表。由于NFS协议的限制，GID列表中最多只能有16个GID。如果使用"-g"
              选项，则从客户端接收到的gid列表将被服务端上的gid列表替换，服务端的gid列表是通过适当
              的搜索查找生成的。注意，主gid(primary gid)不会改变。

TCP_WRAPPERS SUPPORT
       可以使用tcp_wrapper库或iptables来保护rpc.mountd的监听。

       注意，tcp_wrapper只支持IPv4的网络。

       只需向/etc/hosts.allow中添加NFS对端(要求此端rpc.mountd运行被访问)的主机名即可，即使rpc.mountd
       的程序名不是mountd，也要使用mountd作为名称。

       当访问控制文件中使用的主机名不能被解析为IP地址时，该主机名将强制忽略掉。更多信息见tcpd(8)和
       hosts_access(5)的man文档。

   IPv6 and TI-RPC support
       TI-RPC is a pre-requisite for supporting NFS on IPv6.  If TI-RPC support is built into rpc.mountd, 
       it attempts to start listeners on net work  transports  marked 'visible' in /etc/netconfig.As long
       as at least one network transport listener starts successfully, rpc.mountd will operate.

FILES
       /etc/exports        input file for exportfs,listing exports,export options,and access control lists
       /var/lib/nfs/rmtab  table of clients accessing server's exports

SEE ALSO
       exportfs(8), exports(5), showmount(8), rpc.nfsd(8), rpc.rquotad(8), nfs(5), tcpd(8), hosts_access(5),
       iptables(8), netconfig(5)

       RFC 1094 - "NFS: Network File System Protocol Specification"
       RFC 1813 - "NFS Version 3 Protocol Specification"

AUTHOR
       Olaf Kirch, H. J. Lu, G. Allan Morris III, and a host of others.
