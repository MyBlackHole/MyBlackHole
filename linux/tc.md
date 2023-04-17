# tc

## 例子

- 设置网络延迟
```shell
tc qdisc add dev ens192 root netem delay 150ms
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
```

- 列出已有策略
```shell
tc -s qdisc ls dev ens192
tc -q qdisc ls dev ens192
tc qdisc del dev ens192 root
```
