# delete

```
<!-- 强制删除 -->
--force=true --grace-period=0
```

## 例子
- 根据 yaml 配置清除
```shell
kubectl delete -f kube-flannel.yaml
```

- 删除节点
```shell
kubectl delete node <节点名称>
```
