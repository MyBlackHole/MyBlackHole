# ssh

## 配置

- 配置 root 登陆
```shell
sudo nvim /etc/ssh/sshd_config
PermitRootLogin yes
```

```shell
sudo nvim /etc/ssh/sshd_config

ClientAliveInterval 60 # 表示每分钟发送一次, 然后客户端响应, 这样就保持长连接了
ClientAliveCountMax 3 # 使用默认值3即可.ClientAliveCountMax表示服务器发出请求后客户端没有响应的次数达到一定值, 就自动断开

systemctl restart ssh.service

# 多个路径
AuthorizedKeysFile sshd_conf sshd_conf1 sshd_conf2
```

## build
```shell
#  支持 UsePAM
sudo apt-get install libpam0g-dev

# --with-pam 启用 UsePAM
./configure --with-pam CFLAGS='-g -O0' --prefix=/media/black/Data/lib/openssh/openssh-9.3p1
```

## gdb debug
```shell
sudo ssh-keygen -A
sudo /usr/sbin/sshd -f /etc/ssh/sshd_config -D -d -p 1234
```

## 参数
-1: 制使用ssh协议版本1； 
-2 : 强制使用ssh协议版本2； 
-4 : 强制使用IPv4地址； 
-6 : 强制使用IPv6地址； 
-A : 开启认证代理连接转发功能； 
-a : 关闭认证代理连接转发功能； 
-b : 使用本机指定地址作为对应连接的源ip地址； 
-C : 请求压缩所有数据； 
-F : 指定ssh指令的配置文件； 
-f : 后台执行ssh指令； 
-g : 允许远程主机连接主机的转发端口； 
-i : 指定身份文件； 
-l : 指定连接远程服务器登录用户名； 
-N : 不执行远程指令； 
-o : 指定配置选项； 
-p : 指定远程服务器上的端口； 
-q : 静默模式； 
-X : 开启X11转发功能； 
-x : 关闭X11转发功能； 
-y : 开启信任X11转发功能。 


## 例子

- 拷贝公钥到目标机器的授权文件 (authorized_keys)
```shell
ssh-copy-id root@192.168.50.189
```

- 生成 ssh 公钥 私钥
```shell
ssh-keygen -t rsa -C "1358244533@qq.com"
```

- 挂载 ssh 协议文件系统
[[sshfs]]

- 指定密钥文件登陆
```shell
ssh -i ~/.ssh/id_rsa black@127.0.0.1 -p 1234
```
