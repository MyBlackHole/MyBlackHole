# debug

打开应用程序调试功能：

- /usr/sbin/portmap -d
- /usr/sbin/rpc.mountd -d all
- /usr/sbin/rpc.nfsd -d -s

查看NFS配置与记录日志：

- cat /etc/exports
- cat /var/lib/nfs/rmtab
- cat /var/lib/nfs/etab
- cat /var/lib/nfs/xtab
- cat /var/lib/nfs/state
- tail /var/log/messages

### NFS内核模块调试

打开NFS模块调试功能：

- sysctl -w sunrpc.nfs_debug=2147483647
- sysctl -w sunrpc.nfsd_debug=2147483647

查看NFS相关统计和日志：

- cat /proc/slabinfo | grep nfs
- /proc/fs/nfsfs/
- nfsstat
- dmesg

### TCP/IP模块调试

打开RPC模块调试功能：

- sysctl -w sunrpc.rpc_debug=2147483647

查看RPC相关统计和日志：

- cat /var/run/portmap_mapping
- cat /proc/net/rpc/nfs
- cat /proc/net/rpc/nfsd

查看TCP/IP相关统计和配置：

- ping // 查看网络情况
- netstat -tpwn | grep 2049 // 查看NFS TCP链接
- cat /proc/sys/net/ipv4/... // 查看网络配置，如tcp_retries2等
- cat /proc/net/rpc/nfs
- cat /proc/net/rpc/nfsd // deciles等字段
- cat /proc/net/snmp // IP: ReasmFails等字段
- sysctl -a | grep net // 查看所有网络配置参数，如/proc/sys/net/ipv4/tcp_retries2控制tcp断链尝试次数

网络抓包：

- tcpdump -s 9000 -w /tmp/dump.out port 2049