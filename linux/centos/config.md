# config

```shell
# 对于 CentOS 7
sudo sed -e 's|^mirrorlist=|#mirrorlist=|g' \
         -e 's|^#baseurl=http://mirror.centos.org/centos|baseurl=https://mirrors.tuna.tsinghua.edu.cn/centos|g' \
         -i.bak \
         /etc/yum.repos.d/CentOS-*.repo

# 对于 CentOS 8
sudo sed -e 's|^mirrorlist=|#mirrorlist=|g' \
         -e 's|^#baseurl=http://mirror.centos.org/$contentdir|baseurl=https://mirrors.tuna.tsinghua.edu.cn/centos|g' \
         -i.bak \
         /etc/yum.repos.d/CentOS-*.repo

sudo yum makecache
```

- 添加源
```shell
cd /etc/yum.repos.d/
mkdir repo_bak
mv *.repo repo_bak/

wget http://mirrors.aliyun.com/repo/Centos-7.repo
wget http://mirrors.163.com/.help/CentOS7-Base-163.repo

yum clean all
yum makecache
yum install -y epel-release


wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
```

- 防火墙
```shell
systemctl status firewalld.service
```

- 设置界面启动
```shell
<!-- 图形界面 -->
systemctl set-default graphical.target

<!-- 字符界面 -->
systemctl set-default multi-user.target

<!-- 串口终端 -->
<!-- 查询串口终端 -->
$ dmesg | grep tty

<!-- 串口设备开机自启 -->
systemctl enable serial-getty@ttyS0.service

<!-- 配置串口终端参数 -->
vi /etc/serial.conf
/dev/ttyS0 115200,8,n,1

<!-- systemctl set-default serial-getty@ttyS0.service -->
vi /etc/default/grub

GRUB_CMDLINE_LINUX="rhgb quiet"
<!-- to  -->
GRUB_CMDLINE_LINUX="console=ttyS0,115200n8"
GRUB_CMDLINE_LINUX="console=ttyS0,115200"

grub2-mkconfig -o /boot/grub2/grub.cfg
```

- 网络配置
```shell
# 静态 IP
vi /etc/sysconfig/network-scripts/ifcfg-ens33
DEVICE=ens33
BOOTPROTO=static
IPADDR=192.168.1.100
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
DNS1=172.16.58.3
DNS2=8.8.8.8

# 动态 IP
vi /etc/sysconfig/network-scripts/ifcfg-ens3
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=dhcp
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens3
UUID=79dd7b3c-3379-4d9b-8fd6-203221bc66b5
DEVICE=ens3
ONBOOT=yes # 开机启动

# 重启网络
systemctl restart network
```


