# ip
配置网络
在 Linux 系统启动时，内核会为路由策略数据库配置三条缺省的规则： 

0：匹配任何条件，查询路由表local(ID 255)，该表local是一个特殊的路由表，包含对于本地和广播地址的优先级控制路由。rule 0非常特殊，不能被删除或者覆盖。 
32766：匹配任何条件，查询路由表main(ID 254)，该表是一个通常的表，包含所有的无策略路由。系统管理员可以删除或者使用另外的规则覆盖这条规则。 
32767：匹配任何条件，查询路由表default(ID 253)，该表是一个空表，它是后续处理保留。对于前面的策略没有匹配到的数据包，系统使用这个策略进行处理，这个规则也可以删除。 

注：不要混淆路由表和策略：规则指向路由表，多个规则可以引用一个路由表，而且某些路由表可以策略指向它。如果系统管理员删除了指向某个路由表的所有规则，这个表没有用了，但是仍然存在，直到里面的所有路由都被删除，它才会消失

linux 系统中，可以自定义从 1－252个路由表，其中，linux系统维护了4个路由表： 
0#表： 系统保留表 
253#表： defulte table 没特别指定的默认路由都放在改表 
254#表： main table 没指明路由表的所有路由放在该表 
255#表： locale table 保存本地接口地址，广播地址、NAT地址 由系统维护，用户不得更改 

路由表序号和表名的对应关系在 /etc/iproute2/rt_tables

### link
网卡配置
show : 显示（默认） 
set : 设置 
get : 查看 
add : 添加 
del : 删除 
help : 帮助 

### address
配置地址.相当于 ifconfig
show : 显示（默认） 
set : 设置 
get : 查看 
add : 添加 
del : 删除 
help : 帮助 

### route
配置路由器,相当于 route
show : 显示（默认） 
set : 设置 
get : 查看 
add : 添加 
del : 删除 
help : 帮助 


## 例子
- 添加、删除一个ip地址 
注意⚠️ 接口是否启用 

要机器设置一个IP地址 
ip addr add 192.168.0.193/24 dev wlan0 
删除IP地址 
ip addr del 192.168.0.193/24 dev wlan0 

add
sudo ip addr add 192.168.0.193/24 dev enp3s0f1 

del 
sudo ip addr del 192.168.0.193/24 dev enp3s0f1 


- 查看、删除、添加路由 
ip route add default via 192.168.1.1 table 1 
sudo ip route add 192.168.1.0/24 dev ppp0 

在一号表中添加默认路由为192.168.1.1 
ip route add default via 192.168.1.1 

在一号表中添加一条到192.168.0.0网段的路由 ip为192.168.1.2 
ip route add 192.168.0.0/24 via 192.168.1.2 table 1  

查看序号为table-number 的表 
ip route show table table_number 

查看table-name的表 
ip route show table table_name 

添加一个默认路由 
ip route add default gw 20.0.0.1 

添加一个路由表，eth X是10.0.1所在的网卡, 3 是路由表的编号 
ip route add table 3 via 10.0.0.1 dev ethX 

添加 ip rule 规则,fwmark 3 是标记，table 3 是路由表3 上边。 意思就是凡事标记了 3 的数据使用 table3 路由表 
ip rule add fwmark 3 table 3 

iptables 给相应的数据打上标记 
iptables -A PREROUTING -t mangle -i eth0 -s 192.168.0.1 - 192.168.0.100 -j MARK --set-mark 3 

show 
ip route show  

get 
ip route get 192.168.42.0 

del（存在） 
sudo ip route del default dev wlp4s0 

add（不存在） 
sudo ip route add default via 192.168.43.1 dev wlp4s0 

通过路由表 inr.ruhep 路由 源地址为192.203.80/24的数据包 
ip rule add from 192.203.80/24 table inr.ruhep prio 220 

把源地址为193.233.7.83的数据报的源地址转换为192.203.80.144，并通过表1进行路由 
ip rule add from 193.233.7.83 nat 192.203.80.144 table 1 prio 320 

- 启用、关闭接口 
down（关） 
sudo ip link set enp3s0f1 down 

up（开） 
sudo ip link set enp3s0f1 up 

- 统计数据 
statistics 
ip -s -s link ls p2p1 
显示不同网络接口的统计数据 

- 显示网卡配置 
ip link show 

- 重命名网络接口 
ip link set eth0 name xxx 

