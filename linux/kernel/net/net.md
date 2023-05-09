# net

## IPVlan
mode 12
mode 13

```shell
ip netns add ns1
ip netns add ns2
# ip netns add ns3
# ip netns add ns4

ip link add ipvlan1 link wlp1s0 type ipvlan mode l2
ip link add ipvlan2 link wlp1s0 type ipvlan mode l2
# ip link add ipvlan3 link enx0203040d3136 type ipvlan mode l2
# ip link add ipvlan4 link enx0203040d3136 type ipvlan mode l2

ip link set ipvlan1 netns ns1
ip link set ipvlan2 netns ns2
# ip link set ipvlan3 netns ns3
# ip link set ipvlan4 netns ns4

ip netns exec ns1 ip addr add 192.168.100.181/24 dev ipvlan1
ip netns exec ns2 ip addr add 192.168.100.182/24 dev ipvlan2
# ip netns exec ns3 ip addr add 192.168.100.183/24 dev ipvlan3
# ip netns exec ns4 ip addr add 192.168.100.184/24 dev ipvlan4

ip netns exec ns1 ip link set lo up
ip netns exec ns2 ip link set lo up
# ip netns exec ns3 ip link set lo up
# ip netns exec ns4 ip link set lo up

ip netns exec ns1 ip link set ipvlan1 up
ip netns exec ns2 ip link set ipvlan2 up
# ip netns exec ns3 ip link set ipvlan3 up
# ip netns exec ns4 ip link set ipvlan4 up

## (192.168.100.1 为宿主机默认网关)
ip netns exec ns1 ip route add default via 192.168.100.1 
ip netns exec ns2 ip route add default via 192.168.100.1
# ip netns exec ns3 ip route add default via 192.168.100.1
# ip netns exec ns4 ip route add default via 192.168.100.1
```
