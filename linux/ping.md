# ping
测试远程主机

## -c
ping 次数

## -b <广播地址>
ping 整个网段

## -s <封包大小>
默认 64 字节

## 例子
```shell
ping -c4 192.168.31.5 
```

- 指定本地源 ip 或指定网络接口设备
```shell
ping -I 192.168.100.179 baidu.com
ping -I wlp1s0 baidu.com
```
