# ssh

## 配置
```shell
# 都个路径
AuthorizedKeysFile sshd_conf sshd_conf1 sshd_conf2
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
