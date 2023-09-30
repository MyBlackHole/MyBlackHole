# kubeadm

[https://kubernetes.io/zh/docs/reference/setup-tools/kubeadm/](https://kubernetes.io/zh/docs/reference/setup-tools/kubeadm/) 

用于初始化cluster的一个工具

## init

创建mater节点 
```shell
kubeadm init \
  --apiserver-advertise-address=192.168.43.43 \
  --image-repository registry.aliyuncs.com/google_containers \
  --service-cidr=10.96.0.0/12 \
  --pod-network-cidr=10.244.0.0/16
```


## reset

节点重置

## config

|   |   |
|---|---|
|kubeadm config images list|镜像列表|
|kubeadm config images pull|拉取镜像|


## token

|   |   |
|---|---|
|kubeadm token create --print-join-command|创建token|
|kubeadm token create --ttl 0 --print-join-command|生成一个永不过期的token|
|kubeadm token list|查看token列表|

