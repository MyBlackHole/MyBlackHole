# networkctl
网络管理

## 参数
status （link） :查看网络配置信息 

## 配置
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

