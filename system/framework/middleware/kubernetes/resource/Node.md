# node

- 允许所有节点可调度
```shell
kubectl taint nodes --all node-role.kubernetes.io/control-plane-
```

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
Annotations:        csi.volume.kubernetes.io/nodeid: {"rook-ceph.cephfs.csi.ceph.com":"node1","rook-ceph.rbd.csi.ceph.com":"node1"}
                    flannel.alpha.coreos.com/backend-data: {"VNI":1,"VtepMAC":"c6:89:47:dd:4d:0c"}
                    flannel.alpha.coreos.com/backend-type: vxlan
                    flannel.alpha.coreos.com/kube-subnet-manager: true
                    flannel.alpha.coreos.com/public-ip: 192.168.78.212
                    kubeadm.alpha.kubernetes.io/cri-socket: unix:///var/run/containerd/containerd.sock
                    node.alpha.kubernetes.io/ttl: 0
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Wed, 01 Nov 2023 11:10:59 +0800
Taints:             <none>
Unschedulable:      false
Lease:
  HolderIdentity:  node1
  AcquireTime:     <unset>
  RenewTime:       Thu, 02 Nov 2023 16:19:26 +0800
Conditions:
  Type                 Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----                 ------  -----------------                 ------------------                ------                       -------
  NetworkUnavailable   False   Thu, 02 Nov 2023 15:45:59 +0800   Thu, 02 Nov 2023 15:45:59 +0800   FlannelIsUp                  Flannel is running on this node
  MemoryPressure       False   Thu, 02 Nov 2023 16:16:38 +0800   Wed, 01 Nov 2023 11:10:57 +0800   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure         False   Thu, 02 Nov 2023 16:16:38 +0800   Wed, 01 Nov 2023 11:10:57 +0800   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure          False   Thu, 02 Nov 2023 16:16:38 +0800   Wed, 01 Nov 2023 11:10:57 +0800   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready                True    Thu, 02 Nov 2023 16:16:38 +0800   Wed, 01 Nov 2023 11:11:12 +0800   KubeletReady                 kubelet is posting ready status
Addresses:
  InternalIP:  192.168.78.212
  Hostname:    node1
Capacity:
  cpu:                4
  ephemeral-storage:  52194408Ki
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             7989852Ki
  pods:               110
Allocatable:
  cpu:                4
  ephemeral-storage:  48102366334
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             7887452Ki
  pods:               110
System Info:
  Machine ID:                 c8a04de21c22457dbd8d4e9ccc7b45bd
  System UUID:                8E221D42-7C8D-57BF-E61E-0FD52A3DE5E6
  Boot ID:                    34439646-c974-418f-bdb8-e70a44307832
  Kernel Version:             3.10.0-1160.88.1.el7.x86_64
  OS Image:                   CentOS Linux 7 (Core)
  Operating System:           linux
  Architecture:               amd64
  Container Runtime Version:  containerd://1.6.18
  Kubelet Version:            v1.28.2
  Kube-Proxy Version:         v1.28.2
PodCIDR:                      10.244.0.0/24
PodCIDRs:                     10.244.0.0/24
Non-terminated Pods:          (11 in total)
  Namespace                   Name                               CPU Requests  CPU Limits  Memory Requests  Memory Limits  Age
  ---------                   ----                               ------------  ----------  ---------------  -------------  ---
  kube-flannel                kube-flannel-ds-wqnrv              100m (2%)     0 (0%)      50Mi (0%)        0 (0%)         34m
  kube-system                 coredns-5dd5756b68-68tlq           100m (2%)     0 (0%)      70Mi (0%)        170Mi (2%)     29h
  kube-system                 coredns-5dd5756b68-6hvrs           100m (2%)     0 (0%)      70Mi (0%)        170Mi (2%)     29h
  kube-system                 etcd-node1                         100m (2%)     0 (0%)      100Mi (1%)       0 (0%)         29h
  kube-system                 kube-apiserver-node1               250m (6%)     0 (0%)      0 (0%)           0 (0%)         29h
  kube-system                 kube-controller-manager-node1      200m (5%)     0 (0%)      0 (0%)           0 (0%)         29h
  kube-system                 kube-proxy-qznpl                   0 (0%)        0 (0%)      0 (0%)           0 (0%)         29h
  kube-system                 kube-scheduler-node1               100m (2%)     0 (0%)      0 (0%)           0 (0%)         29h
  rook-ceph                   csi-cephfsplugin-b75c4             0 (0%)        0 (0%)      0 (0%)           0 (0%)         2m4s
  rook-ceph                   csi-rbdplugin-smkjk                0 (0%)        0 (0%)      0 (0%)           0 (0%)         2m4s
  rook-ceph                   rook-ceph-mon-c-bff8c5c7f-2452c    0 (0%)        0 (0%)      0 (0%)           0 (0%)         43s
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
