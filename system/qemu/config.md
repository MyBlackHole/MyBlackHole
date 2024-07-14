# config
[连接](https://wiki.gentoo.org/wiki/QEMU/Options#Machine)

- cpu
```shell
# cpu type
-machine type=pc-i440fx-2.1,accel=kvm
# cpu cores
-smp cores=2
# cpu sockets
-smp sockets=1
```

- memory
```
# memory size
-m 2G
```


- network
```
<!-- user  -->
-net nic -net user (默认就是这个, 虚拟机可以连接外面，外面无法连接虚拟机包括宿主机器)

<!-- 动态 ip -->
lvim etc/network/interface
auto lo
iface lo inet loopback
auto eth0
iface eth0 inet dhcp

systemctl restart network

<!-- 静态 ip -->
auto eth0
iface eth0 inet static
address 192.168.0.192
netmask 255.255.255.0
broadcast 192.168.1.255
gateway 192.168.1.1


<!-- bridge (不支持 wifi) -->
-nic tap,model=e1000

<!-- 宿主机器 -->
<!-- 宿主机端创建网桥 -->
#!/bin/sh
#sudo ifconfig eth0 down                 # 首先关闭宿主机网卡接口
sudo brctl addbr br0                     # 添加一座名为 br0 的网桥
sudo brctl addif br0 eth0                # 在 br0 中添加一个接口
sudo brctl stp br0 off                   # 如果只有一个网桥，则关闭生成树协议
sudo brctl setfd br0 1                   # 设置 br0 的转发延迟
sudo brctl sethello br0 1                # 设置 br0 的 hello 时间
sudo ifconfig br0 0.0.0.0 promisc up     # 启用 br0 接口
sudo ifconfig eth0 0.0.0.0 promisc up    # 启用网卡接口
sudo dhclient br0                        # 从 dhcp 服务器获得 br0 的 IP 地址
sudo brctl show br0                      # 查看虚拟网桥列表
sudo brctl showstp br0                   # 查看 br0 的各接口信息

<!-- 创建一个 TAP 设备，作为 QEMU 一端的接口 -->
#!/bin/sh
ip tuntap add dev tap0 mode tap
<!-- sudo tunctl -t tap0 -u root              # 创建一个 tap0 接口，只允许 root 用户访问 -->
sudo brctl addif br0 tap0                # 在虚拟网桥中增加一个 tap0 接口
sudo ifconfig tap0 0.0.0.0 promisc up    # 启用 tap0 接口
sudo brctl showstp br0

<!-- 此时宿主机的一端网卡与qemu一端的网卡都连入了网桥 -->


```

- storage
```
# storage device
-drive file=path/to/image.img,if=virtio

qemu-img create -f raw block.img 320M
-hda block.img
-hdb block.img
-hdc block.img
```
