# brctl

```shell
# brctl addbr <name>     ## 创建一个名为 name 的桥接网络接口
# brctl delbr <name>     ## 删除一个名为 name 的桥接网络接口，桥接网络接口必须先 down 掉后才能删除
# brctl show             ## 显示目前所有的桥接接口

#  brctl addif <brname> <ifname>  
	## 把一个物理接口 ifname 加入桥接接口 brname 中，所有从 ifname 收到的帧都将被 brname 处理
	## 就像网桥处理的一样。所有发往 brname 的帧，ifname 就像输出接口一样。当物理以太网接口加入网桥后，处于混杂模式了，所以不需要配置IP
# brctl delif <brname> <ifname>    ## 从 brname 中脱离一个ifname接口
# brctl show <brname>              ## 显示一些网桥的信息



# brctl addbr brneo           ## 创建新网桥 brneo
# brctl delbr brneo           ## 删除网桥 brneo
# brctl addif brneo eth0      ## 将接口 eth0 加入网桥 brneo
# brctl delif brneo eth0      ## 将接口 eth0 从网桥 brneo 中删除
# brctl show                  ## 查看网桥信息
# brctl show brneo            ## 查看网桥 brneo 的信息  
# brctl stp brneo on          ## 开启网桥 brneo 的 STP，避免成环
```


```shell
brctl addbr br0
brctl addif br0 tap0
brctl show

brctl addif br0 eth0

```
