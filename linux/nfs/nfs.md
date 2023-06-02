# nfs 

## 安装
```
yum install nfs-utils
yum install rpcbind
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
/aiopool 192.168.78.0/24(rw)
systemctl reload nfs

showmount -e 192.168.78.214
Export list for 192.168.78.214:
/aiopool 192.168.78.0/24

mkdir ~/aiopool
mount -t nfs 192.168.78.214:/aiopool ~/aiopool

# 或
nvim /etc/fstab
192.168.78.214:/aiopool  /root/aiopool       nfs    defaults 0 0
mount -a
```
