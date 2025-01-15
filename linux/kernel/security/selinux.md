# selinux

- 查询开启状态
```shell
/usr/sbin/sestatus -v
SELinux status: enabled ## 开启状态
```

- 临时关闭
```shell
<!-- 设置SELinux 成为permissive模式 -->
setenforce 0
--
setenforce Permissive
--
echo 0 > /selinux/enforce
```

- 临时开启
```shell
<!-- 设置SELinux 成为enforcing模式 -->
setenforce 1
```

- 永久开启
```shell
<!-- 编辑/etc/selinux/config文件，将SELINUX=disabled改为SELINUX=enforcing -->
vi /etc/selinux/config
SELINUX=enforcing
```

- 永久关闭
```shell
<!-- 编辑/etc/selinux/config文件，将SELINUX=enforcing改为SELINUX=disabled -->
vi /etc/selinux/config
SELINUX=disabled
```


## 会造成的问题
```shell
<!-- 内核打开文件时会 -->
Enter a hex number: fffffffffffffff3
error: -13.
error str: Permission denied.
```


- ftp 测试
[[ftp]]
```shell
[root@localhost ~]# ps axuZ|grep ftp
system_u:system_r:ftpd_t:s0-s0:c0.c1023 root 1584 0.0  0.0 53292  576 ?        Ss   10:52   0:00 /usr/sbin/vsftpd /etc/vsftpd/vsftpd.cf
```
