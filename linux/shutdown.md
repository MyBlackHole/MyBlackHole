# shutdown
关闭计算机
![[imgs/Pasted image 20230428023006.png]]

## 例子
- 定时关机, 2 小时
```shell
shutdown -h +120 
```

- 取消关机设置
```shell
shutdown -c
```

- 查看设置
```shell
shutdown --show
Shutdown scheduled for Fri 2023-06-16 02:27:10 CST, use 'shutdown -c' to cancel.
```
