# netstat
网络工具

## install
```shell
sudo apt install net-tools
```

## 例子
- 查询端口
```shell
netstat -ln4
```

- 查询 连接队列 溢出情况
```shell
netstat -s | grep -i "listen"
    157455 times the listen queue of a socket overflowed
    157455 SYNs to LISTEN sockets dropped

分别表示有多少 TCP socket 链接因为全连接队列、半连接队列满了而被丢弃：
157455 times the listen queue of a socket overflowed 代表有 157455 次全连接队列溢出
157455 SYNs to LISTEN sockets dropped 代表有 157455 次半连接队列溢出

如果一段时间内相关数值一直在上升，则表明半连接队列、全连接队列有溢出情况
```
