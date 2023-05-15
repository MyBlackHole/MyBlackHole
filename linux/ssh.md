# ssh

## 配置
```shell
sudo nvim /etc/ssh/sshd_config

ClientAliveInterval 60 # 表示每分钟发送一次, 然后客户端响应, 这样就保持长连接了
ClientAliveCountMax 3 # 使用默认值3即可.ClientAliveCountMax表示服务器发出请求后客户端没有响应的次数达到一定值, 就自动断开

systemctl restart ssh.service
```

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
