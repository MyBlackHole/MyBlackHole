### 配置
```shell
Redis 的配置信息在 /etc/redis/redis.conf 下。 

绑定 ip：如果需要远程访问，可将此⾏注释，或绑定⼀个真实 ip 
bind 127.0.0.1 

端⼝，默认为 6379 
添加认证，设置密码 
requirepass redis123456 

受保护模式 
protected-mode no 

是否以守护进程运⾏ 
如果以守护进程运⾏，则不会在命令⾏阻塞，类似于服务 
如果以⾮守护进程运⾏，则当前终端被阻塞 
设置为 yes 表示守护进程，设置为 no 表示⾮守护进程 
推荐设置为 yes 
daemonize yes 

rdb数据⽂件名 
dbfilename dump.rdb 

⽇志⽂件 
logfile /var/log/redis/redis-server.log 

数据库，默认有 16 个 
database 16 

主从复制，类似于双机备份。 
slaveof 

配置aof、rdb文件存放路径 
dir /path 

aof 开启/关闭 
appendonly yes/no 

aof文件名 
appendfilename “xxx.oaf”  
```
