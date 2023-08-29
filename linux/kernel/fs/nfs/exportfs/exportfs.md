# exportfs
NFS共享管理
用于管理 NFS(Network File System) 文件系统
在不直接编辑 /etc/exports 文件的情况下
可用exportfs来操作

## 格式
exportfs [必要参数][选择参数][目录]

### 选择参数
```shell
-a 递增式更新,对/etc/exports 增加或修改的部分进行挂载和卸载
-i<文件> 指定配置文件
-r 更新配置,重新读取/etc/exports
-u 卸载指定目录
-o 使用指定参数
-v 显示共享详细情况
```

- 常用参数 
```shell
ro 只读访问
rw 读写访问
sync 同步写入硬盘
async 暂存内存
secure NFS通过1024以下的安全TCP/IP端口发送
insecure NFS通过1024以上的端口发送
wdelay 多个用户对共享目录进行写操作时，则按组写入数据（默认）
no_wdelay
多个用户对共享目录进行写操作时，则立即写入数据
hide 不共享其子目录
no_hide 共享其子目录
subtree_check 强制NFS检查父目录的权限
no_subtree_check 不检查父目录权限
all_squash 任何访问者，都转为 匿名yong
root_squash root用户访问此目录， 映射成如anonymous用户一样的权限（默认）
no_root_squash root用户访问此目录，具有root操作权限
```

## 例子
- 显示简明共享情况
```shell
exportfs
```

- 显示详细共享情况
```shell
exportfs -v

/home/snail/share/qte
192.168.1.15 /25(rw,wdelay,root_squash, no_subtree_check)
//192.168.1.15 /25 这段IP对该目录具有读写权限

/home/snail/share/tslib
192.168.1.15 /25(rw,wdelay,root_squash, no_subtree_check)
//192.168.1.15 的IP对该目录具有读写权限

/usr/local
(rw,wdelay, insecure, no_root_squash, no_subtree_check)
//所有用户均可访问对该目录具有读写权限

/home/snail/share/tslib
(rw,wdelay, insecure, no_root_squash, no_subtree_check)
//所有用户 均可访问对该目录但只有只读权限
```

- 使 /etc/exports 配置生效
```shell
sudo exportfs -r

sudo exportfs -rv
exporting */0:/home/black/Documents

# client
mount 192.168.100.109:/home/black/Documents /mnt/mounted/
```

- 添加共享目录，且有只读权限
```shell
root@lx138.com~# exportfs *:/home/snail/share/qtmpk
//所有用户均可访问对该目录但只有只读权限

root@lx138.com~# exportfs -o async *:/home/snail/share/gtk_1
//所有用户均可访问对该目录但只有只读权限，且允许匿名访问

root@lx138.com~# exportfs -o async 192.168.1.15:/home/snail/share/gtk_2/
192.168.1.15 的IP可访问对该目录但只有只读权限，且允许匿名访问
```
