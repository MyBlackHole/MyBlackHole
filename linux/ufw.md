# 防火墙

## 配置
```shell
Docker 配置使其不修改防火墙 
{ 
  "iptables": false 
} 
```

## 例子
- 删除规则
```shell
sudo ufw delete 7 # 编号7
sudo ufw delete allow 53
```

- 查询规则状态
```shell
sudo ufw status numbered # numbered 显示编号
```

- 拒绝 外部访问
```shell
sudo ufw deny smtp
```

- 允许特定 ip
```shell
sudo ufw allow from 192.168.254.254
```
- 其他
```shell
启用
sudo ufw enable

外来访问默认拒绝
sudo ufw default deny

外来访问默认允许
sudo ufw default allow

关闭
sudo ufw disable

查看状态
sudo ufw status

禁止外部访问smtp服务  
sudo ufw deny smtp

删除上面建立的某条规则
sudo ufw delete allow smtp

阻止ip地址
sudo ufw deny from 15.15.15.51

阻止ip到特定的网络接口
sudo ufw deny in on eth0 from 15.15.15.51

要拒绝所有的TCP流量从10.0.0.0/8 到 192.168.0.1 地址的22端口
sudo ufw deny proto tcp from 10.0.0.0/8 to 192.168.0.1 port 22

禁止外部访问80端口
sudo ufw delete allow 80 

许所有ssh连接 
sudo ufw allow ssh 
sudo ufw allow 22 

允许此IP访问所有的本机端口
sudo ufw allow from 192.168.1.1 :  

允许特定地址或子网连接rsync
sudo ufw allow from15.15.15.0/24 to any port 873

可以允许所有RFC1918网络（局域网/无线局域网的）访问这个主机（/8,/16,/12是一种网络分级） 
sudo ufw allow from 10.0.0.0/8 
sudo ufw allow from 172.16.0.0/12 
sudo ufw allow from 192.168.0.0/16 
```
