# flannel
功能是让集群中的不同节点主机创建的Docker容器都具有全集群唯一的虚拟IP地址。
![[imgs/img_v2_211dea37-2b5d-4456-a1e7-1d1174f8ac0g.webp]]
![[imgs/img_v2_d94abbd0-99d2-4534-b2e9-192413a1d8eg.webp]]![[imgs/Pasted image 20231023121905.png]]
## install
```shell
kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml
```
