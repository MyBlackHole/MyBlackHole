# sshfs
挂载 ssh 协议文件系统

## 安装
```shell

sudo apt install sshfs
```
## 配置
```shell
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
