# netlink
用户进程与内核进程通信方式

## 例子
- 查看 netlink 使用情况
eth：为netlink的协议号
pid：0，代表内核使用，非零值代表是进程号
```shell
cat /proc/net/netlink
```
