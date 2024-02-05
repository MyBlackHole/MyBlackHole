# describe

查看详细信息

## 例子
- 查看节点详细信息
```shell
kubectl describe nodes node1
Name:               node1
Roles:              <none>
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=node1
                    kubernetes.io/os=linux
Annotations:        node.alpha.kubernetes.io/ttl: 0
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Thu, 19 Oct 2023 09:28:07 +0800
Taints:             <none>
Unschedulable:      false
Lease:
  HolderIdentity:  node1
  AcquireTime:     <unset>
  RenewTime:       Thu, 19 Oct 2023 09:48:52 +0800
Conditions:
  Type             Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----             ------  -----------------                 ------------------                ------                       -------
  MemoryPressure   False   Thu, 19 Oct 2023 09:48:12 +0800   Thu, 19 Oct 2023 09:28:06 +0800   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure     False   Thu, 19 Oct 2023 09:48:12 +0800   Thu, 19 Oct 2023 09:28:06 +0800   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure      False   Thu, 19 Oct 2023 09:48:12 +0800   Thu, 19 Oct 2023 09:28:06 +0800   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready            True    Thu, 19 Oct 2023 09:48:12 +0800   Thu, 19 Oct 2023 09:47:52 +0800   KubeletReady                 kubelet is posting ready status
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
Non-terminated Pods:          (5 in total)
  Namespace                   Name                             CPU Requests  CPU Limits  Memory Requests  Memory Limits  Age
  ---------                   ----                             ------------  ----------  ---------------  -------------  ---
  kube-system                 etcd-node1                       100m (2%)     0 (0%)      100Mi (1%)       0 (0%)         20m
  kube-system                 kube-apiserver-node1             250m (6%)     0 (0%)      0 (0%)           0 (0%)         20m
  kube-system                 kube-controller-manager-node1    200m (5%)     0 (0%)      0 (0%)           0 (0%)         20m
  kube-system                 kube-flannel-ds-2rtfg            100m (2%)     100m (2%)   50Mi (0%)        50Mi (0%)      85s
  kube-system                 kube-scheduler-node1             100m (2%)     0 (0%)      0 (0%)           0 (0%)         20m
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests    Limits
  --------           --------    ------
  cpu                750m (18%)  100m (2%)
  memory             150Mi (1%)  50Mi (0%)
  ephemeral-storage  0 (0%)      0 (0%)
  hugepages-1Gi      0 (0%)      0 (0%)
  hugepages-2Mi      0 (0%)      0 (0%)
Events:
  Type     Reason                   Age                From             Message
  ----     ------                   ----               ----             -------
  Warning  InvalidDiskCapacity      20m                kubelet          invalid capacity 0 on image filesystem
  Normal   NodeHasSufficientMemory  20m (x8 over 20m)  kubelet          Node node1 status is now: NodeHasSufficientMemory
  Normal   NodeHasNoDiskPressure    20m (x7 over 20m)  kubelet          Node node1 status is now: NodeHasNoDiskPressure
  Normal   NodeHasSufficientPID     20m (x7 over 20m)  kubelet          Node node1 status is now: NodeHasSufficientPID
  Normal   NodeAllocatableEnforced  20m                kubelet          Updated Node Allocatable limit across pods
  Normal   RegisteredNode           20m                node-controller  Node node1 event: Registered Node node1 in Controller
  Normal   NodeReady                62s                kubelet          Node node1 status is now: NodeReady
```
