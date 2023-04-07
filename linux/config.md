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

- ubuntu 开机自动登陆用户
```shell
sudo nvim /etc/gdm3/custom.conf
# 修改
AutomaticLoginEnable = true
AutomaticLogin = <user name>
```

- 给文件或文件夹加锁、解锁
```shell
# 加锁
chattr +i 1.txt

# 解锁
chattr -i 1.txt
```

- 安装自签证 SSL 证书
```shell
# 上传根证书(Root CA)到 /usr/local/share/ca-certificates/ (需要使用.crt扩展名)
sudo nvim /usr/local/share/ca-certificates/feitsui.crt
# 更新 CA certificates
sudo update-ca-certificates
# Root CA应该出现在 /etc/ssl/certs/ca-certificates.crt 底部。
tail /etc/ssl/certs/ca-certificates.crt -n 50

```
