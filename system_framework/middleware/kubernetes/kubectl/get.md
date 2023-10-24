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

- 节点详情
```shell
kubectl describe nodes node1
Name:               node1
Roles:              control-plane
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=node1
                    kubernetes.io/os=linux
                    node-role.kubernetes.io/control-plane=
                    node.kubernetes.io/exclude-from-external-load-balancers=
Annotations:        flannel.alpha.coreos.com/backend-data: {"VNI":1,"VtepMAC":"ae:a8:fa:6a:6f:07"}
                    flannel.alpha.coreos.com/backend-type: vxlan
                    flannel.alpha.coreos.com/kube-subnet-manager: true
                    flannel.alpha.coreos.com/public-ip: 192.168.78.212
                    kubeadm.alpha.kubernetes.io/cri-socket: unix:///var/run/containerd/containerd.sock
                    node.alpha.kubernetes.io/ttl: 0
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Thu, 19 Oct 2023 11:27:13 +0800
Taints:             node-role.kubernetes.io/control-plane:NoSchedule (污点:处理调度定制)
Unschedulable:      false
Lease:
  HolderIdentity:  node1
  AcquireTime:     <unset>
  RenewTime:       Tue, 24 Oct 2023 11:04:39 +0800
Conditions:
  Type                 Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----                 ------  -----------------                 ------------------                ------                       -------
  NetworkUnavailable   False   Thu, 19 Oct 2023 11:32:22 +0800   Thu, 19 Oct 2023 11:32:22 +0800   FlannelIsUp                  Flannel is running on this node
  MemoryPressure       False   Tue, 24 Oct 2023 11:02:01 +0800   Thu, 19 Oct 2023 11:27:12 +0800   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure         False   Tue, 24 Oct 2023 11:02:01 +0800   Thu, 19 Oct 2023 11:27:12 +0800   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure          False   Tue, 24 Oct 2023 11:02:01 +0800   Thu, 19 Oct 2023 11:27:12 +0800   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready                True    Tue, 24 Oct 2023 11:02:01 +0800   Thu, 19 Oct 2023 11:27:13 +0800   KubeletReady                 kubelet is posting ready status
Addresses:
  InternalIP:  192.168.78.212
  Hostname:    node1
Capacity:
  cpu:                4
  ephemeral-storage:  52194408Ki
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             7989844Ki
  pods:               110
Allocatable:
  cpu:                4
  ephemeral-storage:  48102366334
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             7887444Ki
  pods:               110
System Info:
  Machine ID:                 c8a04de21c22457dbd8d4e9ccc7b45bd
  System UUID:                8E221D42-7C8D-57BF-E61E-0FD52A3DE5E6
  Boot ID:                    da1e2db8-2aee-4290-a8d6-63bb0d223a92
  Kernel Version:             3.10.0-1160.88.1.el7.x86_64
  OS Image:                   CentOS Linux 7 (Core)
  Operating System:           linux
  Architecture:               amd64
  Container Runtime Version:  containerd://1.6.18
  Kubelet Version:            v1.28.2
  Kube-Proxy Version:         v1.28.2
PodCIDR:                      10.244.0.0/24
PodCIDRs:                     10.244.0.0/24
Non-terminated Pods:          (9 in total)
  Namespace                   Name                             CPU Requests  CPU Limits  Memory Requests  Memory Limits  Age
  ---------                   ----                             ------------  ----------  ---------------  -------------  ---
  kube-flannel                kube-flannel-ds-g8ljd            100m (2%)     0 (0%)      50Mi (0%)        0 (0%)         4d23h
  kube-system                 coredns-5dd5756b68-5lltw         100m (2%)     0 (0%)      70Mi (0%)        170Mi (2%)     4d23h
  kube-system                 coredns-5dd5756b68-vwqmf         100m (2%)     0 (0%)      70Mi (0%)        170Mi (2%)     4d23h
  kube-system                 etcd-node1                       100m (2%)     0 (0%)      100Mi (1%)       0 (0%)         4d23h
  kube-system                 kube-apiserver-node1             250m (6%)     0 (0%)      0 (0%)           0 (0%)         4d23h
  kube-system                 kube-controller-manager-node1    200m (5%)     0 (0%)      0 (0%)           0 (0%)         4d23h
  kube-system                 kube-proxy-hqz4t                 0 (0%)        0 (0%)      0 (0%)           0 (0%)         4d23h
  kube-system                 kube-scheduler-node1             100m (2%)     0 (0%)      0 (0%)           0 (0%)         4d23h
  metallb-system              speaker-jsk7z                    0 (0%)        0 (0%)      0 (0%)           0 (0%)         4d18h
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests    Limits
  --------           --------    ------
  cpu                950m (23%)  0 (0%)
  memory             290Mi (3%)  340Mi (4%)
  ephemeral-storage  0 (0%)      0 (0%)
  hugepages-1Gi      0 (0%)      0 (0%)
  hugepages-2Mi      0 (0%)      0 (0%)
Events:              <none>
```
