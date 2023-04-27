# setcap
添加Capabilities权限

## 例子
- 添加使用网络原始套接字权限
```shell
setcap 'cap_net_admin,cap_net_raw+ep' /bin/ping
```