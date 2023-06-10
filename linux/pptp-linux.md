# pptp-linux

## install
```shell
sudo apt-get install pptp-linux 
```

## 配置
```shell
# 配置命令
sudo pptpsetup --create schoolAllow --server 服务器IP或域名 --username 账号 --password 密码 --encrypt --start --maxfail 0 

# maxfail 0 表示断线后一直尝试重连 

pptpsetup --create NAME --server ADDRESS --username DOMAIN\USER --password PWD --encrypt –start 
   #--encrypt：支持加密，若系统不支持需要加载ppp_mope模块 
   #--start：创建后马上连接 

ip route delete default 
ip route add default dev ppp0 

sudo pptpsetup --create mypptp --server xxx.xxx.xxx.xxx --username NNNN --password PPPP --encrypt --start 
sudo ip route add 192.168.1.0/24 dev ppp0 
sudo pon mypptp  启动mypptp  
sudo poff mypptp 关闭 mypptp 
```
