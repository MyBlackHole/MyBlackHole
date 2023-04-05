# lsof
系统管理、安全工具


## 选项
默认 : 没有选项，lsof列出活跃进程的所有打开文件
组合 : 可以将选项组合到一起，如-abc，但要当心哪些选项需要参数
-a : 结果进行“与”运算（而不是“或”）
-l : 在输出显示用户ID而不是用户名
-h : 获得帮助
-t : 仅获取进程ID
-U : 获取UNIX套接口地址
-F : 格式化输出结果，用于其它命令。可以通过多种方式格式化，如-F pcfn（用于进程id、命令名、文件描述符、文件名，并以空终止）


## 例子
- 显示连接
```shell
# lsof -i[46] [protocol][@hostname|hostaddr][:service|port]

# 所有连接
lsof -i

# 获取 ipv6 流量
lsof -i 6
```

- 显示 TCP 连接
```shell
lsof -i:22
```

- 显示指定到指定主机的连接
```shell
lsof -i@172.16.12.5
```

- 显示基于主机与端口的连接
```shell
lsof  -i@172.16.12.5:22
```

- 找出正在等待连接的端口
```shell
lsof -i -sTCP:LISTEN
```

- 找出已建立的连接
```shell
lsof  -i -sTCP:ESTABLISHED
```

- 显示指定用户打开了什么
```shell
lsof -u black

# 显示 black 以外的其他用户
lsof -u ^black
```

- 查看指定的命令正在使用的文件和网络
```shell
lsof -c syslog-ng
```

- 查看指定进程已打开内容
```shell
lsof -p 1000
```

- 显示指定目录或文件的交互的所有一切
```shell
lsof /var/log
```
