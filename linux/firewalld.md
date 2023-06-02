# firewalld


## 操作
systemctl status firewalld

查看版本： firewall-cmd --version
查看帮助： firewall-cmd --help
显示状态： firewall-cmd --state
查看所有打开的端口： firewall-cmd --zone=public --list-ports
更新防火墙规则： firewall-cmd --reload
查看区域信息:  firewall-cmd --get-active-zones
查看指定接口所属区域： firewall-cmd --get-zone-of-interface=eth0
拒绝所有包：firewall-cmd --panic-on
取消拒绝状态： firewall-cmd --panic-off
查看是否拒绝： firewall-cmd --query-panic

- 开放端口
firewall-cmd --add-service=http --permanent
sudo firewall-cmd --add-port=2888/tcp --permanent
sudo firewall-cmd --add-port=3888/tcp --permanent

–permanent永久生效，没有此参数重启后失效

firewall-cmd --reload

firewall-cmd --list-all

