# ssh


## 例子

- 拷贝公钥到目标机器的授权文件 (authorized_keys)
```shell
ssh-copy-id root@192.168.50.189
```

- 生成 ssh 公钥 私钥
```shell
ssh-keygen -t rsa -C "1358244533@qq.com"
```

- 挂载 ssh 协议文件系统
```shell

sudo apt install sshfs

# 挂载
mkdir /tmp/db1_bin
sshfs root@192.168.78.202:/home/goldendb/db1/bin/ /tmp/db1_bin
# 取消挂载
umount /tmp/db1_bin/

# 设置只读依赖于 sftp-server 
## 将整个sftp-server子系统设置成只读模式
Subsystem sftp /usr/lib/ssh/sftp-server -R

# 细节[https://saucer-man.com/operation_and_maintenance/480.html]
```
