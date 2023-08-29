# nfsd
rpc.nfsd 程序实现的是用户空间层的 NFS 服务，主要功能由 nfsd 内核模块来处理
用户空间程序主要指定内核空间层服务监听在哪个套接字上，使用多少个内核线程。
```c
// 依赖于 内核 nfsd 文件系统挂载出来的 路径
#define NFSD_FS_DIR	  "/proc/fs/nfsd"
#define NFSD_PORTS_FILE   NFSD_FS_DIR "/portlist"
#define NFSD_VERS_FILE    NFSD_FS_DIR "/versions"
#define NFSD_THREAD_FILE  NFSD_FS_DIR "/threads"
```

```shell
pwd
/proc/fs/nfsd

sudo cat portlist
tcp 2049
tcp 2049

sudo cat versions
+3 +4 +4.1 +4.2

sudo cat threads
8
```

rpc.mountd 服务提供辅助服务，用于处理 NFS 客户端的 mount 请求。

## 参数
```shell
-d  or  --debug
             开启调试日志。
-H  or  --host hostname
       默认情况下，rpc.nfsd将接受所有已知网络接口地址上的NFS请求，使用该选项可以指定一个
       特定的主机名(或IP地址)，使得nfsd仅接受对该主机名(或IP)的NFS请求。注意lockd(提供NFS
       文件锁服务的程序)仍接受所有已知网络接口上的请求，在未来的Kernel中可能会做出改变。
       可以指定多次该选项，使得nfsd监听在多个接口上实现nfs多实例。
-p  or  --port port
       指定nfsd用于接受NFS请求的监听端口。默认rpc.nfsd监听端口为2049。
-r  or  --rdma
       指定所有标准RDMA端口(nfsrdma,端口20049)上的NFS请求应该被处理。
       (译者注：RDMA(Remote Direct Memory Access)可以几乎不需要cpu的参与，直接把数据复制
       到远程机器上指定地址内存空间的操作，将替代传统DMA模式)
--rdma=port
       开启一个备用端口监听RDMA请求 - 端口是/etc/services中列出的某项。
-N  or  --no-nfs-version vers
       该选项让rpc.nfsd不向特定版本的NFS提供服务。当前rpc.nfsd支持的NFS版本有2,3,4以及最新
       的4.1。
-s  or  --syslog
       默认情况下，rpc.nfsd会将错误信息(如果开启了debug信息，还包括debug信息)记录到stderr。
       该选项让rpc.nfsd将信息记录到syslog中。注意，处理选项时遇到错误，仍会输出到stderr。
-T  or  --no-tcp
       禁止rpc.nfsd接收客户端发起的TCP连接。
-U  or  --no-udp
       禁止rpc.nfsd接收客户端发起的UDP连接。
-V  or  --nfs-version vers
       该选项让rcp.nfsd向特定版本NFS提供服务。当前rpc.nfsd支持的NFS版本有2,3,4以及最新的4.1。
-L  or  --lease-time seconds
       设置NFSv4的租约时间(lease-time)。它的意思是客户端多久需要向server端确认一次它们的状态。
       有效范围为10-3600秒。
-G  or  --grace-time seconds
       设置NFSv4的grace-time，NFSv2和NFSv3的NLM。新文件打开请求(NFSv4)和新的文件锁(NLM)在这段
       期间不被允许，直到这段时间过去后客户端才能恢复状态，才能继续打开新文件或申请文件锁。
nproc  
       指定NFS服务的线程数。默认仅启动一个线程，但应该指定多个线程以确保其最佳性能。实际数
       量应该取决于NFS客户端的数量和它们带来的负载程度，但8个线程是一个不错的线程数量起点。
       使用nfsstat(8)工具可以测试修改线程数带来的影响。

如果NFS服务处于已运行状态，则指定host,port,protocol的选项将被忽略，仅有指定线程数的选项才会
生效，且会调整(增加或减少)当前的线程数以匹配所指定的值，尤其要注意的是指定0个线程时将会停止
所有线程，并因此会关闭所有打开的连接。
```

## run 过程
```shell
rpcbind
statd
idmapd
nfsdcld
mountd
nfsd
sm-notify


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


systemctl start nfs-server.service
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
    100024    1    udp       0.0.0.0.207.143        status     134
    100024    1    tcp       0.0.0.0.167.119        status     134
    100024    1    udp6      ::.170.203             status     134
    100024    1    tcp6      ::.148.169             status     134
    100005    1    udp       0.0.0.0.224.220        mountd     superuser
    100005    1    tcp       0.0.0.0.206.201        mountd     superuser
    100005    1    udp6      ::.149.151             mountd     superuser
    100005    1    tcp6      ::.173.65              mountd     superuser
    100005    2    udp       0.0.0.0.193.168        mountd     superuser
    100005    2    tcp       0.0.0.0.202.231        mountd     superuser
    100005    2    udp6      ::.152.93              mountd     superuser
    100005    2    tcp6      ::.146.217             mountd     superuser
    100005    3    udp       0.0.0.0.176.228        mountd     superuser
    100005    3    tcp       0.0.0.0.237.113        mountd     superuser
    100005    3    udp6      ::.151.73              mountd     superuser
    100005    3    tcp6      ::.179.117             mountd     superuser
    100003    3    tcp       0.0.0.0.8.1            nfs        superuser
    100003    4    tcp       0.0.0.0.8.1            nfs        superuser
    100227    3    tcp       0.0.0.0.8.1            nfs_acl    superuser
    100003    3    tcp6      ::.8.1                 nfs        superuser
    100003    4    tcp6      ::.8.1                 nfs        superuser
    100227    3    tcp6      ::.8.1                 nfs_acl    superuser
    100021    1    udp       0.0.0.0.228.55         nlockmgr   superuser
    100021    3    udp       0.0.0.0.228.55         nlockmgr   superuser
    100021    4    udp       0.0.0.0.228.55         nlockmgr   superuser
    100021    1    tcp       0.0.0.0.158.177        nlockmgr   superuser
    100021    3    tcp       0.0.0.0.158.177        nlockmgr   superuser
    100021    4    tcp       0.0.0.0.158.177        nlockmgr   superuser
    100021    1    udp6      ::.153.45              nlockmgr   superuser
    100021    3    udp6      ::.153.45              nlockmgr   superuser
    100021    4    udp6      ::.153.45              nlockmgr   superuser
    100021    1    tcp6      ::.130.95              nlockmgr   superuser
    100021    3    tcp6      ::.130.95              nlockmgr   superuser
    100021    4    tcp6      ::.130.95              nlockmgr   superuser
```
