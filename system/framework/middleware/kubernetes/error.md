# error

- Default StorageClass was not found
```shell
kubectl create -f - <<EOF
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
name: local-storage
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: WaitForFirstConsumer
EOF


kubectl apply -f - <<EOF
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
name: local-pve
spec:
accessModes:
- ReadWriteOnce
resources:
requests:
storage: 50Gi
storageClassName: local-storage
EOF
```

- 无法拉取镜像 registry.k8s.io/kube-apiserver:v1.27.1
```shell
设置代理
```

- validate service connection: validate CRI v1 image API for endpoint
```shell
sudo nvim /etc/containerd/config.toml
# disabled_plugins = ["cri"]
```

- The connection to the server localhost:8080 was refused - did you specify the right host or port?
```shell
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

- no matches for kind "PodSecurityPolicy" in version "policy/v1beta1"
```shell
版本变更了,请换合适版本
```

- node NotReady
```shell
<!-- CNI 网络配置 -->
kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml
```

- error execution phase preflight:
couldn't validate the identity of the API Server: 
configmaps "cluster-info" is forbidden: 
User "system:anonymous" cannot get resource "configmaps" in API group "" in the namespace "kube-public"
```shell
aioserver init 配置异常
重装集群
```

- Failed to create SubnetManager: error retrieving pod spec for 'kube-flannel/kube-flannel-ds-kdb4d': Get "https://10.96.0.1:443/api/v1/namespaces/kube-flannel/pods/kube-flannel-ds-kdb4d": dial tcp 10.96.0.1:443: i/o timeout
- CrashLoopBackOff
```shell

```

- [preflight] Running pre-flight checks 卡住不动
```shell
<!-- 可能时间不同步 -->
yum install ntpdate
ntpdate ntp1.aliyun.com;hwclock --systohc

加上 --v=5 看具体问题
```

- Failed to request cluster-info, will try again: configmaps "cluster-info" not found
```shell
aioserver init 配置异常
重装集群
```


- "Unable to update cni config" err="no networks found in /etc/cni/net.d"
```shell
场景一 : Master 出现该问题 , flannel 可能版本和 K8S 不匹配 , 我使用的 K8S 为 1.21 , 重新安装 0.13 的 flannel 后正常 
场景二 : node 出现该问题 , 原因为 node 节点缺少 cni @ blog.csdn.net/u010264186/… 

复制 master cni 文件到 node 中  : scp -r master:/etc/cni /etc/
重启 : systemctl daemon-reload && systemctl restart kubelet

核心关键在 cni 文件的创建  ,成功后会在 /etc/cni/net.d 下出现一个 10-flannel.conflist 文件夹
```


- 排除 init 失败问题
```shell

// Step 1 : 查看 Docker 运行情况 
docker ps -a | grep kube | grep -v pause

// 这个环节可以看到异常的实例 , 如下就是 etcd 和 API server 出现了问题
"etcd --advertise-cl…"   40 seconds ago   Exited (1) 39 seconds ago  
"kube-apiserver --ad…"   39 seconds ago   Exited (1) 18 seconds ago 

---------------------

// Step 2 : 查看 Pod 对应的 log 及 查看 kubelet log
docker logs ac266e3b8189
journalctl -xeu kubelet

// 这里可以看到最终的问题详情
api-server : connection error: desc = "transport: Error while dialing dial tcp 127.0.0.1:2379: connect: connection refused". Reconnecting
// PS : 了解到 127.0.0.1:2379 是 etcd 的端口 (81.888.888.888 是我服务器的 IP )
etc-server : listen tcp 81.888.888.888:2380: bind: cannot assign requested address

---------------------

// Step 3 ：这里很明显就是 etcd 的问题了 , 解决问题 (查找资料后判断是 etcd 的问题 )
修改 etcd 的配置文件 /etc/kubernetes/manifests/etcd.yml , 将 IP 修改为 0.0.0.0 , 也就是没做任何限制
- --listen-client-urls=https://0.0.0.0:2379,https://0.0.0.0:2379（修改的位置）
- --listen-peer-urls=https://0.0.0.0:2380（修改的位置）


---------------------

// Step 4 : 备份上一步的 etcd.yml , 重置 K8S
kubeadm reset

// PS : 重置的过程中 , 会将 manifests 中的东西删除 , 此处记得要取出备份

---------------------

// Step 5 : 替换文件 
- 重新初始化集群
- 当/etc/kubernetes/manifests/etcd.yaml被创建出来时 , 迅速将etcd.yaml文件删除
- 将重置节点之前保存的etcd.yaml文件移动到/etc/kubernetes/manifests/目录

// PS : 操作完成后 , init 还在下载镜像 , 后续就安装成功


// 补充命令 :
- 重启 kubelet : systemctl restart kubelet.service
- 查看 kubelet 日志 : journalctl -xeu kubelet
- 查看 kubelet 状态 : systemctl status kubelet
- 查看 Docker 运行情况 : docker ps -a | grep kube | grep -v pause
- 查看 log : docker logs ac266e3b8189
- 获取所有的 node 节点 :　kubectl get nodes



```


- detected "cgroupfs" as the Docker cgroup driver. The recommended driver is "systemd".
```shell

// Step 1 : 问题的判断
- 输出 Group 类型 :　docker info|grep "Cgroup Driver"

// Step 2 : 重置 kubeadm配置
kubeadm reset
//  或者使用 echo y|kubead reset

// Step 3 : 修改 Docker
1. 打开 /etc/docker/daemon.json
2. 添加 "exec-opts": ["native.cgroupdriver=systemd"]
// PS : 没有可以直接创建 , 最终效果如下
{
 "exec-opts":["native.cgroupdriver=systemd"]
}

// Step 4 : 修改 kubelet
cat > /var/lib/kubelet/config.yaml <<EOF
apiVersion: kubelet.config.k8s.io/v1beta1
kind: KubeletConfiguration
cgroupDriver: systemd
EOF

// Step 4 : 重启服务
systemctl daemon-reload
systemctl restart docker
systemctl restart kubelet

// Step  5 : 校验结果 , 应该输出为 systemd
docker info|grep "Cgroup Driver"

// 补充 : 
kubelet 的配置文件 : /var/lib/kubelet/kubeadm-flags.env

```

- Error response from daemon: Head registry-1.docker.io/v2/coredns/…: connection reset by peer
```shell
// 修改 /etc/docker/daemon.json 中镜像的配置 , 可以直接去阿里云中申请
{
 "registry-mirrors":["https://......mirror.aliyuncs.com"]
}

```

- failed to pull image ..../coredns:v1.8.0: output: Error response from daemon: manifest for ...../coredns:v1.8.0 not found: manifest unknown
```shell
// Step 1 : docker 拉取 coredns
docker pull coredns/coredns:1.8.0
docker tag coredns/coredns:1.8.0 registry.cn-hangzhou.aliyuncs.com/google_containers/coredns:v1.8.0

// --- 同时修改 init 的 image-repository 属性 , 例如 (详见 Master 主流程)
kubeadm init --image-repository registry.cn-hangzhou.aliyuncs.com/google_containers ....
```

- configmaps "cluster-info" is forbidden: User "system:anonymous" cannot get resource "configmaps" in API group "" in the namespace "kube-public"
```shell

kubectl create clusterrolebinding test:anonymous --clusterrole=cluster-admin --user=system:anonymous

<!-- 正式环境解决 : TODO -->
```

- failed to pull image k8s.gcr.io/kube-proxy:v1.21.2: output: Error response from daemon
```shell
kubeadm config images pull --image-repository=registry.aliyuncs.com/google_containers
```

- default-scheduler  0/3 nodes are available: pod has unbound immediate PersistentVolumeClaims. preemption: 0/3 nodes are available: 3 Preemption is not helpful for scheduling..
[[StorageClass]]
```shell
修改chart 文件中的pvc 取值， 让storageClass=现有的storageclass name
或者设置默认存储
```

- FATA[0000] validate service connection: validate CRI v1 image API for endpoint "unix:///run/containerd/containerd.sock": rpc error: code = Unimplemented desc = unknown service runtime.v1.ImageService
```shell
sudo nvim /etc/containerd/config.toml
<!-- 注释 cri -->
# disabled_plugins = ["cri"]

sed -i -r '/cri/s/(.*)/#\1/' /etc/containerd/config.toml
systemctl restart containerd
```
