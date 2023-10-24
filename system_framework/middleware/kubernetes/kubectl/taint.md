# taint

- 禁止master部署pod命令
```shell
kubectl taint nodes master node-role.kubernetes.io/master=true:NoSchedule
```
