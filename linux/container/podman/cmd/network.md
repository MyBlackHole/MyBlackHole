# network

- 将容器连接到网络
```shell
podman network connect mynetwork mycontainer
```

- 将容器连接到网络并指定 IP 地址
```shell
podman network connect --ip 192.168.1.10 mynetwork mycontainer
```

- 将容器连接到网络并设置别名
```shell
podman network connect --alias webserver mynetwork mycontainer
```

- 创建新的网络 macvlan 模式
```shell
sudo podman network create -d macvlan -o parent=enp2s0f0 --subnet 192.5.0.0/16 newnet
```
