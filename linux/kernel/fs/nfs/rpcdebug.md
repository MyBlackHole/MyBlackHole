# rpcdebug


```shell
rpcdebug -m nfs -c
rpcdebug -m nfs -s all
rpcdebug -m nfs -s proc
rpcdebug -m nfs -s file
rpcdebug -m nfs -s dircache
rpcdebug -m nfs -s pagecache
rpcdebug -m nfs -s fscache
rpcdebug -m nfs -s client
rpcdebug -m nfs -s state
rpcdebug -m nfs -s xdr
rpcdebug -m nfs -s vfs
dmesg
```

```shell
mount -t debugfs nodev /sys/kernel/debug

echo 1 > /sys/kernel/debug/tracing/events/fs/enable
echo 1 > /sys/kernel/debug/tracing/events/filelock/enable

#默认这几个文件没有，需要用一个nfs请求触发，系统会创建这几个文件
echo 1 > /sys/kernel/debug/tracing/events/nfs/enable
echo 1 > /sys/kernel/debug/tracing/events/nfs4/enable
echo 1 > /sys/kernel/debug/tracing/events/sunrpc/enable

echo 1 > /sys/kernel/debug/tracing/tracing_on
echo 0 > /sys/kernel/debug/tracing/tracing_on

cat /sys/kernel/debug/tracing/trace
echo > /sys/kernel/debug/tracing/trace
```

```shell
echo 65535 > /proc/sys/sunrpc/nfs_debug
dmesg | grep call_start
```

```shell
echo 65535 > /proc/sys/sunrpc/nfs_debug
echo 65535 > /proc/sys/sunrpc/rpc_debug

/*
 * RPC debug facilities
 */
#define RPCDBG_XPRT     0x0001
#define RPCDBG_CALL     0x0002
#define RPCDBG_DEBUG        0x0004
#define RPCDBG_NFS      0x0008
#define RPCDBG_AUTH     0x0010
#define RPCDBG_BIND     0x0020
#define RPCDBG_SCHED        0x0040
#define RPCDBG_TRANS        0x0080
#define RPCDBG_SVCXPRT      0x0100
#define RPCDBG_SVCDSP       0x0200
#define RPCDBG_MISC     0x0400
#define RPCDBG_CACHE        0x0800
#define RPCDBG_ALL      0x7fff

//如果没有指明类型，默认是RPCDBG_FACILITY，在不同文件里它的定义不一样

/*
 * NFS debug flags
 */
#define NFSDBG_VFS      0x0001
#define NFSDBG_DIRCACHE     0x0002
#define NFSDBG_LOOKUPCACHE  0x0004
#define NFSDBG_PAGECACHE    0x0008
#define NFSDBG_PROC     0x0010
#define NFSDBG_XDR      0x0020
#define NFSDBG_FILE     0x0040
#define NFSDBG_ROOT     0x0080
#define NFSDBG_CALLBACK     0x0100
#define NFSDBG_CLIENT       0x0200
#define NFSDBG_MOUNT        0x0400
#define NFSDBG_FSCACHE      0x0800
#define NFSDBG_PNFS     0x1000
#define NFSDBG_PNFS_LD      0x2000
#define NFSDBG_STATE        0x4000
#define NFSDBG_ALL      0xFFFF

//如果没有指明类型，默认是NFSDBG_FACILITY，在不同文件里它的定义不一样
```
