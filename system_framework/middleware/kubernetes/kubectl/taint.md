# taint

- 禁止master部署pod命令
```shell
kubectl taint nodes master node-role.kubernetes.io/master=true:NoSchedule
```

- 所有节点设置为控制节点(控制面)(不可能作此操作)
```shell
kubectl taint nodes --all node-role.kubernetes.io/master=true:NoSchedule
```

- 允许所有节点作为数据节点调度 pod
```shell
kubectl taint nodes --all node-role.kubernetes.io/control-plane-
```
