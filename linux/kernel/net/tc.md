# tc

## 流量控制处理对象

流量的处理由三种对象控制，它们是：
1. qdisc（排队规则）
2. class（类别）
3. filter（过滤器）

## 例子

- 设置网络延迟
```shell
tc qdisc add dev ens192 root netem delay 150ms
tc qdisc add dev lo root netem delay 15000ms
```

- 设置网卡带宽
```shell
tc qdisc add dev ens192 root tbf rate 500Kbit latency 50ms burst 15kb
```

- 设置丢包率
```shell
tc qdisc add dev ens192 root netem loss 50%
#设置ens192丢包率为50%

# 修改
tc qdisc change dev ens192 root netem loss 50%


sudo tc qdisc change dev lo root netem loss 100%
```

- 列出已有策略
```shell
tc -s qdisc ls dev ens192
tc -q qdisc ls dev ens192
tc qdisc del dev ens192 root


sudo tc qdisc ls dev lo root
qdisc netem 8001: root refcnt 2 limit 1000 delay 15s seed 10798173721010870303
```

- 删除策略
```shell
sudo tc qdisc del dev lo root
```
