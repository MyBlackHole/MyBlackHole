# nfs 

## 安装
```
yum install nfs-utils
yum install rpcbind


apt install nfs-kernel-server
```

## 配置
/etc/exports

/aiopool 192.168.78.0/24
格式： 共享目录的路径 允许访问的NFS客户端（共享权限参数）

参数	作用
ro	只读
rw	读写
root_squash	当NFS客户端以root管理员访问时，映射为NFS服务器的匿名用户
no_root_squash	当NFS客户端以root管理员访问时，映射为NFS服务器的root管理员
all_squash	无论NFS客户端使用什么账户访问，均映射为NFS服务器的匿名用户
sync	同时将数据写入到内存与硬盘中，保证不丢失数据
async	优先将数据保存到内存，然后再写入硬盘；这样效率更高，但可能会丢失数据
subtree_check（默认）：若输出目录是一个子目录，则nfs服务器将检查其父目录的权限。
no_subtree_check ：即使输出目录是一个子目录，nfs服务器也不检查其父目录的权限，这样可以提高效率。

- NFS 端口号
nfsd：端口 2049 
rpcbind：端口 111
mountd：端口 20048
statd：端口 662
lockd：端口 32903

- modules

```shell
lsmod | grep nfs

systemctl status nfs-server.service
lsmod | grep nfs
nfsd                  778240  5
auth_rpcgss           172032  1 nfsd
nfs_acl                16384  1 nfsd
lockd                 131072  1 nfsd
grace                  16384  2 nfsd,lockd
sunrpc                737280  16 nfsd,auth_rpcgss,lockd,nfs_acl
```

## 使用
showmount[[showmount]] 查看 nfs 服务器信息
- 查看 nfs 状态
```shell
systemctl status nfs-server
```

- 重启 nfs

```shell
systemctl restart nfs-server
```

- 设置共享目录

```shell
# 服务端
nvim /etc/exports
## 添加 
/aiopool 192.168.78.0/24(rw,sync,no_subtree_check)
systemctl reload nfs (或 exportfs -r)

showmount -e 192.168.78.214
Export list for 192.168.78.214:
/aiopool 192.168.78.0/24


# 客户端
mkdir ~/aiopool
mount -t nfs 192.168.78.214:/aiopool ~/aiopool
# 或
nvim /etc/fstab
192.168.78.214:/aiopool  /root/aiopool       nfs    defaults 0 0
mount -a
```

![[imgs/Pasted image 20240208100622.png]]