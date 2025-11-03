# error

- Error: configure storage: kernel does not support overlay fs: 
'overlay' is not supported over <unknown> at "/home/black/.local/share/containers/storage/overlay": 
backing file system is unsupported for this graph driver
```shell
paru -S fuse-overlayfs
```

- 容器内无法访问域名
```shell
# 宿主机
echo "nameserver 8.8.8.8" > /etc/resolv.conf
```
