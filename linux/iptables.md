# iptables
真正实现防火墙功能的是netfilter
![[imgs/Pasted image 20230509153444.png]]
![[imgs/Pasted image 20230509150633.png]]
![[imgs/Pasted image 20230509150700.png]]
![[imgs/Pasted image 20230509150712.png]]
### 规则表
```shell
1.filter表——三个链：INPUT、FORWARD、OUTPUT

作用：过滤数据包  内核模块：iptables_filter.

2.Nat表——三个链：PREROUTING、POSTROUTING、OUTPUT

作用：用于网络地址转换（IP、端口） 内核模块：iptable_nat

3.Mangle表——五个链：PREROUTING、POSTROUTING、INPUT、OUTPUT、FORWARD

作用：修改数据包的服务类型、TTL、并且可以配置路由实现QOS内核模块：iptable_mangle(别看这个表这么麻烦，咱们设置策略时几乎都不会用到它)

4.Raw表——两个链：OUTPUT、PREROUTING

作用：决定数据包是否被状态跟踪机制处理  内核模块：iptable_raw

(这个是REHL4没有的，不过不用怕，用的不多)
```
### 规则链
```shell
1.INPUT——进来的数据包应用此规则链中的策略

2.OUTPUT——外出的数据包应用此规则链中的策略

3.FORWARD——转发数据包时应用此规则链中的策略

4.PREROUTING——对数据包作路由选择前应用此链中的规则

#（记住！所有的数据包进来的时侯都先由这个链处理）

5.POSTROUTING——对数据包作路由选择后应用此链中的规则

#（所有的数据包出来的时侯都先由这个链处理）
```

### 规则表之间的优先顺序
```shell
Raw -- mangle -- nat -- filter
```
#### 入站数据流向
```shell
从外界到达防火墙的数据包，先被PREROUTING规则链处理（是否修改数据包地址等），之后会进行路由选择（判断该数据包应该发往何处），如果数据包

的目标主机是防火墙本机（比如说Internet用户访问防火墙主机中的web服务器的数据包），那么内核将其传给INPUT链进行处理（决定是否允许通

过等），通过以后再交给系统上层的应用程序（比如Apache服务器）进行响应。
```
#### 转发数据流向
```shell
来自外界的数据包到达防火墙后，首先被PREROUTING规则链处理，之后会进行路由选择，如果数据包的目标地址是其它外部地址（比如局域网用户通过网 关访问QQ站点的数据包），则内核将其传递给FORWARD链进行处理（是否转发或拦截），然后再交给POSTROUTING规则链（是否修改数据包的地 址等）进行处理。
```
#### 出站数据流向
```shell
防火墙本机向外部地址发送的数据包（比如在防火墙主机中测试公网DNS服务器时），首先被OUTPUT规则链处理，之后进行路由选择，然后传递给POSTROUTING规则链（是否修改数据包的地址等）进行处理。
```
## 语法
```shell
iptables [-t 表名] 命令选项 ［链名］ ［条件匹配］ ［-j 目标动作或跳转］
说明：表名、链名用于指定 iptables命令所操作的表和链，命令选项用于指定管理iptables规则的方式（比如：插入、增加、删除、查看等；条件匹配用于指定对符合什么样 条件的数据包进行处理；目标动作或跳转用于指定数据包的处理方式（比如允许通过、拒绝、丢弃、跳转（Jump）给其它链处理
```
### 管理控制选项
```shell
-A 在指定链的末尾添加（append）一条新的规则
-D  删除（delete）指定链中的某一条规则，可以按规则序号和内容删除
-I  在指定链中插入（insert）一条新的规则，默认在第一行添加
-R  修改、替换（replace）指定链中的某一条规则，可以按规则序号和内容替换
-L  列出（list）指定链中所有的规则进行查看
-E  重命名用户定义的链，不改变链本身
-F  清空（flush）
-N  新建（new-chain）一条用户自己定义的规则链
-X  删除指定表中用户自定义的规则链（delete-chain）
-P  设置指定链的默认策略（policy）
-Z 将所有表的所有链的字节和数据包计数器清零
-n  使用数字形式（numeric）显示输出结果
-v  查看规则表详细信息（verbose）的信息
-V  查看版本(version)
-h  获取帮助（help）
```
### 处理数据包的四种方式
```shell
ACCEPT 允许数据包通过

DROP 直接丢弃数据包，不给任何回应信息

REJECT 拒绝数据包通过，必要时会给数据发送端一个响应的信息。

LOG在/var/log/messages文件中记录日志信息，然后将数据包传递给下一条规则
```
### 
## 例子
- 保存 iptables 规则
```shell
iptables-save > iptables-save.txt
```

- 删除 INPUT 链的第一条规则
```shell
iptables -D INPUT 1
```

- 拒绝所有 ICMP 协议数据包
```shell
iptables -I INPUT -p icmp -j REJECT
```

- 允许防火墙转发除 ICMP 协议以外的所有数据包
```shell
iptables -A FORWARD -p ! icmp -j ACCEPT
# ! 可以将条件取反
```

- 拒绝转发来自 192.168.1.10 主机的数据，允许转发来自 192.168.0.0/24 网段的数据
```shell
iptables -A FORWARD -s 192.168.1.11 -j REJECT

iptables -A FORWARD -s 192.168.0.0/24 -j ACCEPT
# 说明：注意要把拒绝的放在前面不然就不起作用了啊
```

- 丢弃从外网接口（eth1）进入防火墙本机的源地址为私网地址的数据包
```shell
iptables -A INPUT -i eth1 -s 192.168.0.0/16 -j DROP

iptables -A INPUT -i eth1 -s 172.16.0.0/12 -j DROP

iptables -A INPUT -i eth1 -s 10.0.0.0/8 -j DROP
```

- 封堵网段 192.168.1.0/24，两小时后解封。
```shell
iptables -I INPUT -s 10.20.30.0/24 -j DROP

iptables -I FORWARD -s 10.20.30.0/24 -j DROP

# at now 2 hours at> iptables -D INPUT 1 at> iptables -D FORWARD 1
# 说明：这个策略咱们借助crond计划任务来完成，就再好不过了
```

- 只允许管理员从202.13.0.0/16网段使用SSH远程登录防火墙主机。
```shell
iptables -A INPUT -p tcp --dport 22 -s 202.13.0.0/16 -j ACCEPT

iptables -A INPUT -p tcp --dport 22 -j DROP

# 说明：这个用法比较适合对设备进行远程管理时使用，比如位于分公司中的SQL服务器需要被总公司的管理员管理时。
```

- 允许本机开放从TCP端口20-1024提供的应用服务。
```shell
iptables -A INPUT -p tcp --dport 20:1024 -j ACCEPT

iptables -A OUTPUT -p tcp --sport 20:1024 -j ACCEPT
```

- 允许转发来自192.168.0.0/24局域网段的DNS解析请求数据包。
```shell
iptables -A FORWARD -s 192.168.0.0/24 -p udp --dport 53 -j ACCEPT

iptables -A FORWARD -d 192.168.0.0/24 -p udp --sport 53 -j ACCEPT
```

- 禁止其他主机ping防火墙主机，但是允许从防火墙上ping其他主机
```shell
iptables -I INPUT -p icmp --icmp-type Echo-Request -j DROP

iptables -I INPUT -p icmp --icmp-type Echo-Reply -j ACCEPT

iptables -I INPUT -p icmp --icmp-type destination-Unreachable -j ACCEPT
```

- 禁止转发来自MAC地址为00：0C：29：27：55：3F的和主机的数据包
```shell
iptables -A FORWARD -m mac --mac-source 00:0c:29:27:55:3F -j DROP

# 说明：iptables中使用“-m 模块关键字”的形式调用显示匹配。咱们这里用“-m mac –mac-source”来表示数据包的源MAC地址。
```

- 允许防火墙本机对外开放TCP端口20、21、25、110以及被动模式FTP端口1250-1280
```shell
iptables -A INPUT -p tcp -m multiport --dport 20,21,25,110,1250:1280 -j ACCEPT

# 说明：这里用“-m multiport –dport”来指定目的端口及范围
```

- 禁止转发源IP地址为192.168.1.20-192.168.1.99的TCP数据包。
```shell
iptables -A FORWARD -p tcp -m iprange --src-range 192.168.1.20-192.168.1.99 -j DROP

# 说明：此处用“-m –iprange –src-range”指定IP范围。
```

- 禁止转发与正常TCP连接无关的非—syn请求数据包。
```shell
iptables -A FORWARD -m state --state NEW -p tcp ! --syn -j DROP

# 说明：“-m state”表示数据包的连接状态，“NEW”表示与任何连接无关的，新的嘛！
```

- 拒绝访问防火墙的新数据包，但允许响应连接或与已有连接相关的数据包
```shell
iptables -A INPUT -p tcp -m state --state NEW -j DROP

iptables -A INPUT -p tcp -m state --state ESTABLISHED,RELATED -j ACCEPT

# 说明：“ESTABLISHED”表示已经响应请求或者已经建立连接的数据包，“RELATED”表示与已建立的连接有相关性的，比如FTP数据连接等。
```


- 只开放本机的web服务（80）、FTP(20、21、20450-20480)，放行外部主机发住服务器其它端口的应答数据包，将其他入站数据包均予以丢弃处理。
```shell
iptables -I INPUT -p tcp -m multiport --dport 20,21,80 -j ACCEPT

iptables -I INPUT -p tcp --dport 20450:20480 -j ACCEPT

iptables -I INPUT -p tcp -m state --state ESTABLISHED -j ACCEPT

iptables -P INPUT DROP
```

- 对离开的数据包源 ip 192.168.1.0/24 网段数据包进行 SNAT 转换
```shell
# 添加
iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -j MASQUERADE

iptables -t nat -A POSTROUTING -o ens224 -s 192.168.1.0/24 -j MASQUERADE

# 删除
iptables -t nat -D POSTROUTING -s 192.168.1.0/24 -j MASQUERADE
```

- 允许 br0 网桥为输入设备进行网络转发
```shell
# 删除
iptables -t filter -D FORWARD -i br0 -j ACCEPT

# 添加
iptables -t filter -A FORWARD -i br0 -j ACCEPT
```

- 固定 SNAT ip 转换
```shell
iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -j SNAT --to-source 192.168.118.55
```


## MASQUERADE
在iptables中有着和SNAT相近的效果，但也有一些区别，但使用SNAT的时候，出口ip的地址范围可以是一个，也可以是多个，例如：

如下命令表示把所有10.8.0.0网段的数据包SNAT成192.168.5.3的ip然后发出去，

iptables-t nat -A POSTROUTING -s 10.8.0.0/255.255.255.0 -o eth0 -j SNAT --to-source192.168.5.3

如下命令表示把所有10.8.0.0网段的数据包SNAT成192.168.5.3/192.168.5.4/192.168.5.5等几个ip然后发出去

iptables-t nat -A POSTROUTING -s 10.8.0.0/255.255.255.0 -o eth0 -j SNAT --to-source192.168.5.3-192.168.5.5

这就是SNAT的使用方法，即可以NAT成一个地址，也可以NAT成多个地址，但是，对于SNAT，不管是几个地址，必须明确的指定要SNAT的ip，假如当前系统用的是ADSL动态拨号方式，那么每次拨号，出口ip192.168.5.3都会改变，而且改变的幅度很大，不一定是192.168.5.3到192.168.5.5范围内的地址，这个时候如果按照现在的方式来配置iptables就会出现问题了，因为每次拨号后，服务器地址都会变化，而iptables规则内的ip是不会随着自动变化的，每次地址变化后都必须手工修改一次iptables，把规则里边的固定ip改成新的ip，这样是非常不好用的。

MASQUERADE就是针对这种场景而设计的，他的作用是，从服务器的网卡上，自动获取当前ip地址来做NAT。

iptables-t nat -A POSTROUTING -s 10.8.0.0/255.255.255.0 -o eth0 -j MASQUERADE

如此配置的话，不用指定SNAT的目标ip了，不管现在eth0的出口获得了怎样的动态ip，MASQUERADE会自动读取eth0现在的ip地址然后做SNAT出去，这样就实现了很好的动态SNAT地址转换
