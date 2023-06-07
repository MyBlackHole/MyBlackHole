```shell
# 
/proc/net/rpc/auth.unix.ip/channel

echo "nfsd 0.0.0.0 2147483647 -test-client-\n" > /proc/net/rpc/auth.unix.ip/channel



# export rpc 接口
/proc/net/rpc/nfsd.export/channel

echo "-test-client- /home/black/Documents  3 9252 65534 65534 0\n" > /proc/net/rpc/nfsd.export/channel

# 导出记录
/var/lib/nfs/etab

# 特征
/proc/fs/nfsd/export_features
```
