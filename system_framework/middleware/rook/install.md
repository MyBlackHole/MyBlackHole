# install

## install
```shell
git clone --single-branch --branch v1.10.8 https://github.com/rook/rook.git

<!-- 部署 Rook Operator -->
cd rook/deploy/examples
kubectl create -f crds.yaml -f common.yaml -f operator.yaml
# 检查
kubectl -n rook-ceph get pod

<!-- 创建 Rook Ceph 集群 -->
cd rook/deploy/examples
kubectl apply -f cluster.yaml

<!-- 部署 Rook Ceph 工具 -->
cd rook/deploy/examples
kubectl create -f toolbox.yaml

<!-- <!-- mgr 账号 --> -->
<!-- kubectl get pod -n rook-ceph | grep mgr | awk '{print $1}' -->

<!-- 部署 Ceph Dashboard -->
cd rook/deploy/examples
kubectl apply -f dashboard-external-https.yaml

# 获取 dashboard admin 账号的密码
kubectl -n rook-ceph get secret rook-ceph-dashboard-password -o jsonpath="{['data']['password']}" | base64 -d
(wC\yfE-hXeUDyWYC$gk

<!-- 通过 ceph-tool 工具 pod 查看 ceph 集群状态 -->
kubectl exec -it `kubectl get pods -n rook-ceph|grep rook-ceph-tools|awk '{print $1}'` -n rook-ceph -- bash

ceph -s
```

