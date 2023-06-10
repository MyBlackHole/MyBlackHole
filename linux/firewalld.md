# firewalld

## 配置
```xml
/usr/lib/firewalld/zones/public.xml | /etc/firewalld/zones/public.xml 
<?xml version="1.0" encoding="utf-8"?> 
<zone> 
  <short>Public</short> 
  <description>For use in public areas. You do not trust the other computers on networks to not harm your computer. Only selected incoming connections are accepted.</description> 
  <!-- 允许单个IP地址访问本服务器所有端口 --> 
  <rule family="ipv4"> 
    <source address="10.1.1.13/32"/> 
    <accept/> 
  </rule> 
  <!-- 允许IP段访问本服务器所有端口 --> 
  <rule family="ipv4"> 
    <source address="10.1.2.0/24"/> 
    <accept/> 
  </rule> 
  <!-- 允许IP段访问本服务器指定端口 --> 
  <rule family="ipv4"> 
    <source address="10.1.3.0/24"/> 
    <port protocol="tcp" port="22"/> 
    <accept/> 
  </rule> 
  <!-- 允许IP段访问本服务器指定端口范围 --> 
  <rule family="ipv4"> 
    <source address="10.1.4.0/24"/> 
    <port protocol="tcp" port="1000-1200"/> 
    <accept/> 
  </rule> 
  <!-- 禁止指定IP访问本服务器 --> 
  <rule family="ipv4"> 
    <source address="10.1.1.1"/> 
    <reject/> 
  </rule> 
</zone> 
```

## 服务
```shell
systemclt status firewalld : 状态 
systemclt start firewalld  : 启动防火墙 
systemclt stop firewalld  : 关闭防火墙 
systemclt disable firewalld  : 开机自动关闭 
systemclt enable firewalld  : 开机自动启动 
```

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

