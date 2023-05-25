# arping
发起 arp 请求


## 格式
```shell
arping [-AbDfhqUV] [-c count] [-w deadline] [-s source] -I interface destination
```

### 参数
-A：与-U参数类似，但是使用的是ARP REPLY包而非ARP REQUEST包。 
-b：发送以太网广播帧，arping在开始时使用广播地址，在收到回复后使用unicast单播地址。 
-c：发送指定的count个ARP REQUEST包后停止。如果指定了-w参数，则会等待相同数量的ARP REPLY包，直到超时为止。 
-D：重复地址探测模式，用来检测有没有IP地址冲突，如果没有IP冲突则返回0。 
-f：收到第一个响应包后退出。 
-h：显示帮助页。 
-I：用来发送ARP REQUEST包的网络设备的名称。 
-q：quite模式，不显示输出。 
-U：无理由的（强制的）ARP模式去更新别的主机上的ARP CACHE列表中的本机的信息，不需要响应。 
-V：显示arping的版本号。 
-w：指定一个超时时间，单位为秒，arping在到达指定时间后退出，无论期间发送或接收了多少包。在这种情况下，arping在发送完指定的count（-c）个包后并不会停止，而是等待到超时或发送的count个包都进行了回应后才会退出。 
-s：设置发送ARP包的IP资源地址，如果为空，则按如下方式处理： 
    1. DAD模式（-D）设置为0.0.0.0； 
    2. Unsolicited模式（-U）设置为目标地址； 
    3. 其它方式，从路由表计算。

## 例子
- 例1：查看某个IP的MAC地址
```shell
arping 192.168.131.155
```

- 例2：查看某个IP的MAC地址，并指定count数量
```shell
arping -c 1 192.168.131.155
```

- 例3：当有多块网卡的时候，指定特定的设备来发送请求包
```shell
arping -i eth1 -c 1 192.168.131.155
```

- 例4：查看某个IP是否被不同的MAC占用
```shell
arping -d 192.168.131.155
```

- 例5：查看某个MAC地址的IP，要在同一子网才查得到
```shell
arping -c 1 52:54:00:a1:31:89
```

- 例6：确定MAC和IP的对应，确定指定的网卡绑定了指定的IP
```shell
arping -c 1  -T 192.168.131.156  00:13:72:f9:ca:60
```

- 例7：确定IP和MAC对应，确定指定IP绑在了指定的网卡上
```shell
arping -c 1  -t  00:13:72:f9:ca:60 192.168.131.156
```


- 例8：有时候，本地查不到某主机，可以通过让网关或别的机器去查。以下几种形式测了下都可以
```shell
arping   -c 1  -S 10.240.160.1 -s 88:5a:92:12:c1:c1  10.240.162.115

arping   -c 1  -S 10.240.160.1   10.240.162.115
```
