# netns
网络命名空间
```shell
# 查询命名空间列表
ip netns ls

# 创建一个新网络空间
ip netns add ns1

# 网络接口列表
ip link list

# 创建虚拟网络接口
ip link add veth0 type veth peer name veth1
ip link list

# 启用
ip link set ens224.1 up

# 把网卡绑定到网络命名空间(宿主机 ip link list 将无法查到)
ip link set veth1 netns test1
## 或
ip link set wlp1s0.1 netns test1
```


```shell
# 添加网络命名空间
ip netns add test1
# 添加虚拟网卡
ip link set veth1 netns test1
# 将网卡添加到网络命名空间中并激活网卡
ip link set veth1 netns test1
ip netns exec test1 ip link set lo up
ip netns exec test1 ip link set veth1 up
ip netns exec test1 ip addr add 1.1.1.1/24 dev veth1
# 在宿主机添加桥接网络并将宿主机物理网卡和虚拟网卡添加到桥接中
ip link add vmbr0 type bridge
ip link set vmbr0 up
ip link set wlp1s0 master vmbr0
ip link set veth0 master vmbr0
# 另一个主机访问网络命名空间中的IP地址
ping 1.1.1.1
```

- 查询其它网络命名空间
```shell
sudo ip netns exec test1 ip link list
```

```shell
ip netns add ns1

ip link add link ens224 name ens224.1 type vlan id 1
# 设置地址(brd +: brd +表示广播地址后24位全部为1 （192.255.255.255）)
ip addr add 192.168.2.1/24 dev ens224.1 brd +

ip netns exec ns1 ip link set lo up

ip netns exec ns1 ip link set ens224.1 up

ip netns exec ns1 ip addr add 192.168.118.233/24 dev ens224.1

ip netns exec ns1 ip route add default via 192.168.118.1

# 删除默认路由
ip route delete default via 192.168.118.1
```



```shell
ip netns add ns1
brctl addbr br0

ip link add veth-ns1 type veth peer name veth-ns1-br
ip link set veth-ns1 netns ns1
brctl addif br0 veth-ns1-br

ip -n ns1 addr add local 192.168.1.2/24 dev veth-ns1
ip addr add local 192.168.1.1/24 dev br0

ip link set br0 up
ip link set veth-ns1-br up
ip -n ns1 link set veth-ns1 up

## 现在可以访问到 br0 (1921.168.1.1) 网桥了

ip netns exec ns1 ip route add default via 192.168.1.1

## 可以访问宿主机器
[root@worker216 ~]# ip route
default via 192.168.100.1 dev ens192 proto static metric 100
default via 192.168.100.1 dev ens224 proto static metric 101
192.168.0.0/17 dev ens192 proto kernel scope link src 192.168.78.216 metric 100
192.168.0.0/17 dev ens224 proto kernel scope link src 192.168.118.55 metric 101
192.168.1.0/24 dev br0 proto kernel scope link src 192.168.1.1
[root@worker216 ~]# ip netns exec ns1 ping 192.168.78.216
PING 192.168.78.216 (192.168.78.216) 56(84) bytes of data.
64 bytes from 192.168.78.216: icmp_seq=1 ttl=64 time=0.038 ms
64 bytes from 192.168.78.216: icmp_seq=2 ttl=64 time=0.038 ms
64 bytes from 192.168.78.216: icmp_seq=3 ttl=64 time=0.038 ms
^C
--- 192.168.78.216 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 1999ms
rtt min/avg/max/mdev = 0.038/0.038/0.038/0.000 ms

iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -o ens224 -j MASQUERADE
# iptables -t filter -A FORWARD -i br0 -j ACCEPT
```
