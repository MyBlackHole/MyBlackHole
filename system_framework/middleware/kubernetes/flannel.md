# flannel
功能是让集群中的不同节点主机创建的Docker容器都具有全集群唯一的虚拟IP地址。

## install
```shell
kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml
```
