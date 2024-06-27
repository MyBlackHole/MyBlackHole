# network

## install
```shell
yum install network-scripts 
```


- 配置
```shell
网卡 
cd /etc/sysconfig/network-scripts
vim ifcfg-eno16777736

TYPE="Ethernet"
BOOTPROTO="static"
DEFROUTE="yes"
PEERDNS="yes"
PEERROUTES="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_PEERDNS="yes"
IPV6_PEERROUTES="yes"
IPV6_FAILURE_FATAL="no"
NAME="eno16777736"
UUID="0e6ca219-0d2e-4000-8f17-bf7424e46595"
DEVICE="eno16777736"
ONBOOT="yes"
IPADDR=192.168.255.101
GATEWAY=192.168.255.2
NETMASK=255.255.255.0
DNS=192.168.255.1
```




- 单网卡配置多个IP地址
```shell
<!-- 原网卡配置 -->
lvim /etc/sysconfig/network-scripts/ifcfg-ens192
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="static"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="no"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="ens192"
DEVICE=ens192
HWADDR=00:50:56:9d:ac:b2
ONBOOT=yes
USERCTL=no
NETMASK=255.255.128.0
IPADDR=192.168.100.211
PEERDNS=no
GATEWAY="192.168.100.1"
DNS1="223.5.5.5"
DNS2="10.6.3.108"

lvim /etc/sysconfig/network-scripts/route6-ens192
lvim /etc/sysconfig/network-scripts/route-ens192
default via 192.168.100.1


<!-- 新网卡配置 -->
lvim /etc/sysconfig/network-scripts/ifcfg-ens192:0
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="static"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="no"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="ens192:0"
DEVICE="ens192:0"
ONBOOT="yes"
IPADDR="10.6.66.7"
NETMASK="255.255.252.0"
GATEWAY="10.6.67.254"
DNS1="223.5.5.5"
DNS2="10.6.3.108"

配置新网卡配置默认网关
lvim /etc/sysconfig/network-scripts/route-ens192
default via 10.6.67.254
default via 192.168.100.1

lvim /etc/sysconfig/network 
NETWORKING=YES
GATEWAY=10.6.67.254 网关地址


<!-- 配置新 ip 默认网关, 重启后生效 -->
route add default gw 10.6.67.254 dev ens192

service network restart
<!-- 或 -->
/etc/init.d/network restart
```
