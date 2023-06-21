# chronyc 
时间同步服务

## 配置
```shell
# 服务端
lvim /etc/chrony.conf
allow 192.168/16 # 设置允许访问的客户端
local stratum 10
systemctl restart chronyd.service

# 客户端
lvim /etc/chrony.conf
server x.x.x.x iburst
systemctl restart chronyd.service
chronyc sources -v
```

- 
```shell
# centos
yum install -y chrony
# 开启自启动
systemctl enable chronyd
systemctl start chronyd
```
[othor](https://www.cnblogs.com/my-show-time/p/14658895.html)

- 同步
```shell
# -n 显示 ip
chronyc -n sources -v

210 Number of sources = 1

  .-- Source mode  '^' = server, '=' = peer, '#' = local clock.
 / .- Source state '*' = current synced, '+' = combined , '-' = not combined,
| /   '?' = unreachable, 'x' = time may be in error, '~' = time too variable.
||                                                 .- xxxx [ yyyy ] +/- zzzz
||      Reachability register (octal) -.           |  xxxx = adjusted offset,
||      Log2(Polling interval) --.      |          |  yyyy = measured offset,
||                                \     |          |  zzzz = estimated error.
||                                 |    |           \
MS Name/IP address         Stratum Poll Reach LastRx Last sample
===============================================================================
^* 192.168.78.212                3   6    17     5   -440ns[  -83us] +/-   43ms
```
