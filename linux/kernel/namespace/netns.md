# netns
网络命名空间
```shell
# 查询命名空间列表
ip netns ls

# 创建一个新网络空间
ip netns add test1

# 网络接口列表
ip  link list

# 创建虚拟网络接口
ip link add veth0 type veth peer name veth1
ip link list

## 或
# 给具体接口建子接口
sudo ip link add link wlp1s0 name wlp1s0.1 type vlan id 1
# 设置地址(brd +: brd +表示广播地址后24位全部为1 （192.255.255.255）)
ip addr add 192.168.2.1/24 dev wlp1s0.1 brd +
# 启用
ip link set wlp1s0.1 up

# 把网卡绑定到网络命名空间(宿主机 ip link list 将无法查到)
ip link set veth1 netns test1
## 或
sudo ip link set wlp1s0.1 netns test1
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
