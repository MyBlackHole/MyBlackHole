# debug

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

- 测试导出与挂载
```shell
[root@191 ~]# systemctl start nfs-server
[root@191 ~]# exportfs -v
[root@191 ~]# exportfs *:/appDxc/
[root@191 ~]# exportfs -v
/appDxc         <world>(sync,wdelay,hide,no_subtree_check,sec=sys,ro,root_squash,no_all_squash)

[root@192 ~]# mkdir /appDxc_mountpoint/
[root@192 ~]# mount xxxxxxx.191:/appDxc/ /appDxc_mountpoint/
```
