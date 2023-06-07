# flock
通过 shell 脚本管理文件锁。

## 用法
```shell
 flock [选项] <文件|目录> <命令> [<参数>...]
 flock [选项] <文件|目录> -c <命令>
 flock [选项] <文件描述符号码>
```

### 选项
 -s, --shared             获取共享锁
 -x, --exclusive          获取排他锁(默认)
 -u, --unlock             移除锁
 -n, --nonblock           失败而非等待
 -w, --timeout <秒>       等待限定的时间
 -E, --conflict-exit-code <数字>     冲突或超时后的退出代码
 -o, --close              运行命令前关闭文件描述符
 -c, --command <命令>      通过 shell 运行单个命令字符串
 -F, --no-fork            执行命令时不 fork
     --verbose            增加详尽程度

 -h, --help               display this help
 -V, --version            display version


## 例子

- 文件锁使用例子
```shell
flock -xn /tmp/mytest.lock -c 'for i in {1..10}; do echo $(date); sleep 5; done'
```

- 堵塞等待
```shell
flock -x /tmp/mytest.lock -c 'for i in {1..10}; do echo $(date); sleep 5; done'
```
