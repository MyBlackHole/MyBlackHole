# arp
arp指令用来管理系统的arp缓冲区，可以显示、删除、添加静态mac地址
ARP以各种方式操纵内核的ARP缓存
主要选项是清除地址映射项并手动设置
为了调试目的，ARP程序还允许对ARP缓存进行完全转储

## 语法
```shell
arp [-evn]  [-H type]  [-i if]  -a  [hostname]

arp [-v]  [-i if]  -d  hostname [pub]

arp [-v]  [-H type]  [-i if]  -s  hostname  hw_ addr [temp]

arp [-v]  [-H type]  [-i if]  -s  hostname hw_ addr  [netmask nm]  pub

arp [-v]  [-H type]  [-i if]  -Ds  hostname ifa  [netmask nm]  pub

arp [-vnD]  [-H type]  [-i if]  -f  [filename]
```

## 例子
- 查询 映射
```shell
arp
```

- 添加 ip 与 mac 映射
```shell
sudo arp -i wlp3s0 -s 192.168.118.55 00:50:56:9d:1a:df
```

- 删除 arp 记录
```shell
sudo arp -d 192.168.2.19
```
