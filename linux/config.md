# 系统配置

- 修改最大文件打开数
```shell
# centos
nvim /etc/security/limits.conf
添加
* soft nofile 65535
sysctl -p # 使配置生效

# ubuntu
nvim /etc/security/limits.conf
添加
* soft nofile 65535
reboot # 需要注销才生效
```

- 关闭安全子系统
```shell
# 临时关闭
setenforce 0

# 永久生效
cat > /etc/selinux/config << EOF
SELINUX=disabled
SELINUXTYPE=targeted
EOF
```

- 时间同步
```shell
# 设置同步时区
timedatectl set-timezone Asia/Shanghai
# 开启时间自动同步
timedatectl set-ntp yes ?
```
