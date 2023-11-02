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

image-repository : 该参数为镜像地址 ,如果下载慢或者 timeout , 需要重新选择新的地址
kubernetes-version : 当前的版本,  可以去官方查询最新版本
apiserver-advertise-address : 该地址为你的 apiServer 地址 , node 会调用该地址 (该地址需要外部可调)
pod-network-cidr : 指定pod网络的IP地址范围，它的值取决于你在下一步选择的哪个网络网络插件
10.244.0.0/16 : Flannel
192.168.0.0/16 : Calico


kubeadm init \
    --apiserver-advertise-address=192.168.78.212  \
    --ignore-preflight-errors=all \
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
|kubeadm token generate|生成token|


## certs
- 查看证书有效期
```shell
kubeadm certs check-expiration
```

- renew
```shell
kubeadm certs renew all
```

## 例子
- 安装集群
```shell
kubeadm init \
    --apiserver-advertise-address=192.168.78.212  \
    --ignore-preflight-errors=all \
    --service-cidr=10.1.0.0/16 \
    --pod-network-cidr=10.244.0.0/16

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

<!-- 每个子节点运行 -->
kubeadm join 192.168.78.212:6443 --token maepwq.fu0blbpnvy12o50h \
        --discovery-token-ca-cert-hash sha256:ffd3493fd3f1178d507622830a4ea0aa6a0d533c64c01fdb23cd9b40687bf366

################ 一定要经常备份 ETCD ################
```


- 创建连接 join
```shell
kubeadm token create --print-join-command

kubeadm join 192.168.78.212:6443 --token 0xet5a.dldbnfvxvgkaly6x --discovery-token-ca-cert-hash sha256:1946b85b70b3d8a2f99035ebd0f76b66a6e9faf35d3f8bfe9d478c01f1caaf23
```

- 重新生成 token 与 join cmd
```shell
kubeadm token generate
y6h47j.68tlarknmzh6qdih

kubeadm token create y6h47j.68tlarknmzh6qdih  --print-join-command --ttl=0
kubeadm join 192.168.78.212:6443 --token y6h47j.68tlarknmzh6qdih --discovery-token-ca-cert-hash sha256:1946b85b70b3d8a2f99035ebd0f76b66a6e9faf35d3f8bfe9d478c01f1caaf23
```

- 卸载清理集群
```shell
kubectl drain <node name> --delete-emptydir-data --force --ignore-daemonsets
kubeadm reset -f 
iptables -F && iptables -t nat -F && iptables -t mangle -F && iptables -X
ipvsadm -C
kubectl delete node <节点名称>
```
