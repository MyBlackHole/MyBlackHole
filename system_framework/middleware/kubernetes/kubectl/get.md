# get

获取列出一个或多个资源的信息
- 语法
get [(-o|--output=)json|yaml|wide|custom-columns=...|custom-columns-file=...|go-template=...|go-template-file=...|jsonpath=...|jsonpath-file=...] (TYPE [NAME | -l label] | TYPE/NAME ...) [flags]

|   |   |
|---|---|
|kubectl get pods|列出所有运行的Pod信息|
|kubectl get pods -o wide|列出Pod以及运行Pod节点信息|
|kubectl get -o json pod web-pod-13je7|以JSON格式输出一个pod信息|
|kubectl get -f pod.yaml -o json|以“pod.yaml”配置文件中指定资源对象和名称输出JSON格式的Pod信息|
|kubectl get -o template pod/web-pod-13je7 --template={{.status.phase}}|返回指定pod的相位值|
|kubectl get rc,services|列出所有replication controllers和service信息|
|kubectl get rc/web service/frontend pods/web-pod-13je7|按其资源和名称列出相应信息|
|kubectl get all|列出所有不同的资源对象|
|watch kubectl get pod|实时查看pod状态|
|kubectl get all|所有|
|Kubectl get ingress--xxxx|查规则|
|kubectl get deployment nginx -n dev -o yaml --export > test2.yaml|导出yaml文件|
|kubectl get pod coredns-7f89b7bc75-54t76 -n kube-system -o yaml||
|kubectl get namespace|查看所有命名空间|
|kubectl get pods -o wide -A -w|持续查看|
|kubectl get pods,svc|查看服务状态|
|kubectl get ConfigMap|查看配置资源|
|kubectl get clusterinformations -o yaml|输出位yaml格式|
|kubectl get pod {podname} -n {namespace} -o yaml \| kubectl replace --force -f -|启动的是 Pod 对象，那么是无法直接删除或者 scale 到 0 的，但可以通过上面这条命令重启。这条命令的意思是 get 当前运行的 pod 的 yaml声明，并管道重定向输出到 kubectl replace命令的标准输入，从而达到重启的目的|

## secrets
- 查看所有密钥
```shell
kubectl get -n kubernetes-dashboard secrets
```

- 创建密钥
```shell
kubectl create secret tls kubernetes-dashboard-certs --key kube-dashboard.key --cert kube-dashboard.crt -n kubernetes-dashboard
```

## services

kubectl get services


## nodes

|   |   |
|---|---|
|kubectl get nodes|查看所有节点|

```shell
[root@node1 ~]# kubectl get nodes
NAME    STATUS     ROLES    AGE     VERSION
node1   NotReady   <none>   4m53s   v1.28.2
```


## pods

|   |   |
|---|---|
|kubectl get pods|查看所有的pod|
|kubectl get pods -n kube-system|查看运行时容器pod|



## pod

|   |   |
|---|---|
|kubectl get pod pod_name|参看某个pod|
|kubectl get pod pod_name -o yaml|查看某个pod，以yaml格式展示结果|

## events

|   |   |
|---|---|
|kubectl get Events --namespace dss|查看dss命名空间事件|





## 例子

- 获取所有命名空间的 pods
```shell
kubectl get pods --all-namespaces
```

- 指定命名空间
```shell
kubectl get --namespace kube-flannel pods
```

- 查看指定 yaml 信息
```shell
kubectl get -f kubernetes-dashboard.yaml
```
