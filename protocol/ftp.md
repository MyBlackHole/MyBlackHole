# frp

```shell
yum -y install vsftpd

paru -S inetutils
```

```shell
systemctl start   vsftpd    # 启动服务。
systemctl stop    vsftpd    # 停止服务。
systemctl restart vsftpd    # 重启服务。
systemctl status  vsftpd    # 查看服务状态。
systemctl enable  vsftpd    # 启用开机自动动vsftpd服务。
systemctl disable vsftpd    # 禁用开机自动动vsftpd服务。


[root@localhost ~]# systemctl status vsftpd
● vsftpd.service - Vsftpd ftp daemon
   Loaded: loaded (/usr/lib/systemd/system/vsftpd.service; disabled; vendor preset: disabled)
   Active: active (running) since Wed 2024-07-10 10:52:40 CST; 4s ago
  Process: 1583 ExecStart=/usr/sbin/vsftpd /etc/vsftpd/vsftpd.conf (code=exited, status=0/SUCCESS)
 Main PID: 1584 (vsftpd)
   CGroup: /system.slice/vsftpd.service
           └─1584 /usr/sbin/vsftpd /etc/vsftpd/vsftpd.conf

Jul 10 10:52:40 localhost.localdomain systemd[1]: Starting Vsftpd ftp daemon...
Jul 10 10:52:40 localhost.localdomain systemd[1]: Started Vsftpd ftp daemon.
Hint: Some lines were ellipsized, use -l to show in full.
```
