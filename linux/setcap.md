# setcap
添加Capabilities权限

## 例子
- 添加使用网络原始套接字权限
```shell
setcap 'cap_net_admin,cap_net_raw+ep' /bin/ping

```

- 普通用户监听1024以下的端口
```shell
# 给应用的可执行文件赋予 CAP_NET_BIND_SERVICE 权限
sudo setcap cap_net_bind_service=+ep ./aio-oss
```
