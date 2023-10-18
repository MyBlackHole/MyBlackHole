# kubeadm

[https://kubernetes.io/zh/docs/reference/setup-tools/kubeadm/](https://kubernetes.io/zh/docs/reference/setup-tools/kubeadm/) 

用于初始化cluster的一个工具

## init

- 创建mater节点 
```shell
kubeadm init \
  --apiserver-advertise-address=192.168.43.43 \
  --image-repository registry.aliyuncs.com/google_containers \
  --service-cidr=10.96.0.0/12 \
  --pod-network-cidr=10.244.0.0/16



kubeadm init \
--apiserver-advertise-address=192.168.56.1  \
--ignore-preflight-errors=all \
--image-repository registry.aliyuncs.com/google_containers \
--service-cidr=10.1.0.0/16 \
--pod-network-cidr=10.244.0.0/16
```

- 添加节点到集群
```shell
kubeadm join 192.168.56.1:6443 --token arj03m.b6ia8p9f3grzdpqn \
	--discovery-token-ca-cert-hash sha256:33c4fbbf00a0ed0805ebcc95bc7b8dd964411c06c5e68fb80e3013c392299325
```


## reset

节点重置

## config

|   |   |
|---|---|
|kubeadm config images list|需要的镜像列表|
|kubeadm config images pull|拉取镜像|

```shell
mkdir -p /etc/systemd/system/containerd.service.d/
nvim /etc/systemd/system/containerd.service.d/http-proxy.conf
[Service]
Environment="HTTP_PROXY=http://127.0.0.1:1080/"
Environment="HTTPS_PROXY=http://127.0.0.1:1080/"

kubeadm config images pull
```


## token

|   |   |
|---|---|
|kubeadm token create --print-join-command|创建token|
|kubeadm token create --ttl 0 --print-join-command|生成一个永不过期的token|
|kubeadm token list|查看token列表|

