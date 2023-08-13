# ps
进程查看

## install
```shell
sudo apt-get update
sudo apt-get install procps
```
![[imgs/Pasted image 20230428014724.png]]

## 例子
- 获取指定进程执行命令详情,不带头
```shell
ps -p 31709 -o args --no-headers
```

- 显示进程组 id(PGID)、父进程 id(PPID)、会话 id(SID)
```shell
ps ajx
```
