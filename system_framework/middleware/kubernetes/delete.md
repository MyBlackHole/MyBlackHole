# delelte

```shell
<!-- 删除对集群的本地引用 -->
kubectl config delete-cluster 

<!-- 使用适当的凭证与控制平面节点通信 -->
kubectl drain <node name> --delete-emptydir-data --force --ignore-daemonsets

kubeadm reset

iptables -F && iptables -t nat -F && iptables -t mangle -F && iptables -X
ipvsadm -C

<!-- 删除节点 -->
kubectl delete node <节点名称>
```
