# sshfs
挂载 ssh 协议文件系统

## 安装
```shell
<!-- ubuntu -->
sudo apt install sshfs

<!-- centos -->
yum install -y epel-release
yum -y install fuse-sshfs

```

## 参数
-o allow_other         allow access to other users
如果不指定本参数，则只有挂载本文件系统的用户才可以访问；如果指定本参数，则允许系统中的其他用户访问；

-o reconnect           reconnect to server
断链后，会自动重连远程服务器，对于网络不太稳定的环境比较有用；

-o ro
挂载后，文件内容为只读

-o cache_timeout=N     sets timeout for caches in seconds (default: 20)
N是整数，指定缓存的超时时间，以秒为单位；

-o follow_symlinks     follow symlinks on the server
跟踪符号链接，可以让sshfs在文件系统中跟踪访问符号链接对应的文件；

-p PORT                equivalent to '-o port=PORT'
-o port=PORT
指定远程服务器的ssh端口号

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

-o nonempty
```


## 例子

- 允许非空与指定端口
```shell
sshfs -o nonempty,port=2222 black@192.168.78.212:/media/black/Data/Documents/AIO/aio-airflow/airflow/dags/aio_tasks /opt/aio/airflow/lib/python3.6/site-packages/aio_tasks

<!-- 只读，自动重连 -->
sshfs -o ro,reconnect,nonempty,port=2222 black@192.168.78.212:/media/black/Data/Documents/AIO/aio-airflow/airflow/dags/aio_tasks /opt/aio/airflow/lib/python3.6/site-packages/aio_tasks
```
