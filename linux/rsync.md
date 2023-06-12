# rsync

## 参数
```shell
-a(重要) : 包含-rtplgoD选项 

-r(重要) : 同步目录时要加上，类似cp命令的-r选项 

-R : 目录不存在创建R 

-v(重要) : 同步时显示一些信息，让我们知道同步的过程 

-l : 保留软链接 

-L : 同步软链接时会把源文件给同步 

-p : 保持文件的权限属性 

-o : 保持文件的属主 

-g : 保持文件的属组 

-D : 保持设备文件信息 

-t : 保持文件的时间属性 

--delete : 删除DEST中SRC没有的文件 

--exclude : 过滤指定文件，如--exclude "logs" 会把文件名包含logs的文件或者目录过滤掉，不同步 

-p : 显示同步过程，比如速率，比-v更加详细 

-u : 如果DEST中的文件比SRC新，就不会同步 

-z : 传输时压缩 
```

## 配置
```shell
#设置服务器信息提示文件名称，在该文件中编写提示信息 
motd file = /etc/rsyncd.motd 


#开启Rsync数据传输日志功能 

transfer logging = yes 


#设置日志文件名称，可以通过log format参数设置日志格式 

log file = /var/log/rsyncd.log 


#设置Rsync进程保存文件名称 

pid file = /var/run/rsyncd.pid 


#设置锁文件名称 

lock file = /var/run/rsyncd.lock 


#设置服务器监听的端口号，默认是873端口 

port = 873 


#设置进行数据传输所使用的账户名称或ID号，默认使用nobody 

uid = rsync 


#设置进行数据传输所使用的组名称或组ID，默认使用nobody 

gid = rsync 


#设置服务器所监听的IP地址，这里的服务器地址是192.168.30.5 

address = 192.168.30.5 


#设置user chroot为yes后，rsync首先会进行chroot设置，将根映射到path参数路径下，对客户端而言，系统的根就是path参数所指定的路径。但这样做需要root权限，并且在同步符号连接资料时仅会同步名称，而内容不会同步。 

use chroot = no 


#是否允许客户端上传数据，这是设置只读 

read only = yes 


#设置并发连接数，0代表无限制。超出并发数后，如果依然有客户端连接请求，则会收到稍后重试的提示信息。 

max connections = 10 


#模块，Rsync通过模块定义同步的目录，模块以[name] 的形式定义，在Rsync中也可以定义多个模块 

[test] 


#comment定义注释说明字串 

comment = web content 


#同步目录的真实路径通过path指定 

path = /root/rsync_test 


#exclude 可以指定例外的目录，即将/tmp/rsync目录下的某个目录设置不同步数据 

exclude = test/ 


#设置允许连接服务器的用户，账户可以是不存在的用户 

auth users = rsync_backup 


#设置密码验证文件名称，注意该文件的权限要求为只读，建议权限600，仅在设置了auth users 后生效 

secrets file = /etc/rsync.passwd 


#设置允许那些主机可以同步数据，可以是单个IP也可以是一个网段，多个IP之间用空格隔开 

hosts allows=192.168.30.5 192.168.30.0/24 


#设置拒绝所有，除了hosts allows定义的 

hosts deny = * 


#客户端请求显示模块列表，本模块名称是否显示，默认为true 

list = false 

```

## 例子

- 文件同步
```shell
rsync -rvza root@x.x.x.x:/shared/ /shared/ 
```
