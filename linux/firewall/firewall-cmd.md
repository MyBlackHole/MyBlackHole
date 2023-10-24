# firewall-cmd

## 操作
查看版本： firewall-cmd --version
查看帮助： firewall-cmd --help
显示状态： firewall-cmd --state
查看所有打开的端口： firewall-cmd --zone=public --list-ports
更新防火墙规则： firewall-cmd --reload
查看区域信息:  firewall-cmd --get-active-zones
查看指定接口所属区域： firewall-cmd --get-zone-of-interface=eth0
拒绝所有包：firewall-cmd --panic-on
取消拒绝状态： firewall-cmd --panic-off
查看是否拒绝： firewall-cmd --query-panic

```shell
firewall-cmd --zone=public --list-ports
8008/tcp 8009/tcp 8080/tcp
```

- 开放端口
```shell
firewall-cmd --add-service=http --permanent
sudo firewall-cmd --add-port=2888/tcp --permanent
sudo firewall-cmd --add-port=3888/tcp --permanent

<!-- -–permanent: 永久生效，没有此参数重启后失效 -->
```

- 关闭端口
```shell
firewall-cmd --permanent --remove-port=8083-8085/tcp
```

- 重载防火墙
```shell
firewall-cmd --reload
```

- 防火墙详情
```shell
firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: ens192
  sources:
  services: cockpit dhcpv6-client mdns ssh
  ports: 8008/tcp 8009/tcp 8080/tcp
  protocols:
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

