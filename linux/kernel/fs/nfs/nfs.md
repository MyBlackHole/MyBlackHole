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

- 常用选项
```shell
ro                      只读访问 
rw                      读写访问 
sync                    同步写数据 
async                   异步写入数据 
secure                  NFS通过1024以下的安全TCP/IP端口发送 
insecure                NFS通过1024以上的端口发送 
wdelay                  如果多个用户要写入NFS目录，则归组写入（默认） 
no_wdelay               如果多个用户要写入NFS目录，则立即写入，当使用async时，无需此设置。 
hide                    在NFS共享目录中不共享其子目录 
no_hide                 共享NFS目录的子目录 
subtree_check           如果共享/usr/bin之类的子目录时，强制NFS检查父目录的权限（默认） 
no_subtree_check        和上面相对，不检查父目录权限 
all_squash              共享文件的UID和GID映射匿名用户anonymous，适合公用目录。 
no_all_squash           保留共享文件的UID和GID（默认） 
root_squash             root用户的所有请求映射成如anonymous用户一样的权限（默认） 
no_root_squas           root用户具有根目录的完全管理访问权限 
anonuid=xxx             指定NFS服务器/etc/passwd文件中匿名用户的UID 
anongid=xxx             指定NFS服务器/etc/passwd文件中匿名用户的GID 
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



mount -t nfs 10.6.64.152:/ob_data /ob_data
```

![[imgs/Pasted image 20240208100622.png]]
