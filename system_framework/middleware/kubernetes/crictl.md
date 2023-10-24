# crictl
类似 docker

对于 [[ctr]] 的命名空间来说 k8s.io


## config

```shell
<!-- nvim /etc/crictl.yaml -->
# these first two endpoint setting is where you configure crictl to containerd
runtime-endpoint: unix:///run/containerd/containerd.sock
image-endpoint: unix:///run/containerd/containerd.sock
timeout: 2
debug: true
pull-image-on-create: false

```

## pull
- 拉取镜像
```shell
crictl pull registry.k8s.io/pause:3.6
```

## ps
- 查看运行的容器
```shell
crictl ps
DEBU[0000] get runtime connection
DEBU[0000] get image connection
DEBU[0000] ListContainerResponse: [&Container{Id:bb774b389f1b3175be0031d89c7f48249a0e03969eb5d66d44229e19c171b1ff,PodSandboxId:6be9baaff6d509306488c0a0b947877c40d30602d42f535696f056ffec0703fb,Metadata:&ContainerMetadata{Name:etcd,Attempt:0,},Image:&ImageSpec{Image:sha256:73deb9a3f702532592a4167455f8bf2e5f5d900bcc959ba2fd2d35c321de1af9,Annotations:map[string]string{},UserSpecifiedImage:,},ImageRef:sha256:73deb9a3f702532592a4167455f8bf2e5f5d900bcc959ba2fd2d35c321de1af9,State:CONTAINER_RUNNING,CreatedAt:1697678879406886886,Labels:map[string]string{io.kubernetes.container.name: etcd,io.kubernetes.pod.name: etcd-node1,io.kubernetes.pod.namespace: kube-system,io.kubernetes.pod.uid: 2ef92a1c38112dc08bc1a9da2a6e6b14,},Annotations:map[string]string{io.kubernetes.container.hash: 14a1bab,io.kubernetes.container.restartCount: 0,io.kubernetes.container.terminationMessagePath: /dev/termination-log,io.kubernetes.container.terminationMessagePolicy: File,io.kubernetes.pod.terminationGracePeriod: 30,},} &Container{Id:c36f095fa3fb962973a31d902e8f3cd7d2bc48cc937c4f514fcfa207be2b56d7,PodSandboxId:dd335a614184adf22a7efd2d79ec6b821ee016751c6a4507c58ac2938285f513,Metadata:&ContainerMetadata{Name:kube-scheduler,Attempt:0,},Image:&ImageSpec{Image:sha256:7a5d9d67a13f6ae031989bc2969ec55b06437725f397e6eb75b1dccac465a7b8,Annotations:map[string]string{},UserSpecifiedImage:,},ImageRef:sha256:7a5d9d67a13f6ae031989bc2969ec55b06437725f397e6eb75b1dccac465a7b8,State:CONTAINER_RUNNING,CreatedAt:1697678884378626859,Labels:map[string]string{io.kubernetes.container.name: kube-scheduler,io.kubernetes.pod.name: kube-scheduler-node1,io.kubernetes.pod.namespace: kube-system,io.kubernetes.pod.uid: 7eae45a1c1e39154aaa255434e034fc7,},Annotations:map[string]string{io.kubernetes.container.hash: 59807296,io.kubernetes.container.restartCount: 0,io.kubernetes.container.terminationMessagePath: /dev/termination-log,io.kubernetes.container.terminationMessagePolicy: File,io.kubernetes.pod.terminationGracePeriod: 30,},} &Container{Id:edcefbc81a0cafcdec006b8ec46fa4b08499c5d2fc1c462220a22fea9fac6c3f,PodSandboxId:52921175b3502db112b6bc9af68208919a0733938ddede04b6ec0d74414199ea,Metadata:&ContainerMetadata{Name:kube-apiserver,Attempt:0,},Image:&ImageSpec{Image:sha256:cdcab12b2dd16cce4efc5dd43c082469364f19ad978e922d110b74a42eff7cce,Annotations:map[string]string{},UserSpecifiedImage:,},ImageRef:sha256:cdcab12b2dd16cce4efc5dd43c082469364f19ad978e922d110b74a42eff7cce,State:CONTAINER_RUNNING,CreatedAt:1697678885447147383,Labels:map[string]string{io.kubernetes.container.name: kube-apiserver,io.kubernetes.pod.name: kube-apiserver-node1,io.kubernetes.pod.namespace: kube-system,io.kubernetes.pod.uid: 36a72ab0e9b41f94ad717dc86dfd88c0,},Annotations:map[string]string{io.kubernetes.container.hash: c5971698,io.kubernetes.container.restartCount: 0,io.kubernetes.container.terminationMessagePath: /dev/termination-log,io.kubernetes.container.terminationMessagePolicy: File,io.kubernetes.pod.terminationGracePeriod: 30,},} &Container{Id:f081aebf4dd4c987ab72fc85c18d611f7e4c3a3e602fac41c53a1081db79f764,PodSandboxId:172980b2e72154aff1cec706d0976d9a63c9648e822e07b3d6ddd90473ab5a74,Metadata:&ContainerMetadata{Name:kube-controller-manager,Attempt:0,},Image:&ImageSpec{Image:sha256:55f13c92defb1eb854040a76e366da866bdcb1cc31fd97b2cde94433c8bf3f57,Annotations:map[string]string{},UserSpecifiedImage:,},ImageRef:sha256:55f13c92defb1eb854040a76e366da866bdcb1cc31fd97b2cde94433c8bf3f57,State:CONTAINER_RUNNING,CreatedAt:1697678886762258300,Labels:map[string]string{io.kubernetes.container.name: kube-controller-manager,io.kubernetes.pod.name: kube-controller-manager-node1,io.kubernetes.pod.namespace: kube-system,io.kubernetes.pod.uid: e6550577a9aa3e73d1d0dd4eb6ba8294,},Annotations:map[string]string{io.kubernetes.container.hash: 43f66262,io.kubernetes.container.restartCount: 0,io.kubernetes.container.terminationMessagePath: /dev/termination-log,io.kubernetes.container.terminationMessagePolicy: File,io.kubernetes.pod.terminationGracePeriod: 30,},}]
CONTAINER           IMAGE               CREATED             STATE               NAME                      ATTEMPT             POD ID              POD
f081aebf4dd4c       55f13c92defb1       2 minutes ago       Running             kube-controller-manager   0                   172980b2e7215       kube-controller-manager-node1
edcefbc81a0ca       cdcab12b2dd16       2 minutes ago       Running             kube-apiserver            0                   52921175b3502       kube-apiserver-node1
c36f095fa3fb9       7a5d9d67a13f6       2 minutes ago       Running             kube-scheduler            0                   dd335a614184a       kube-scheduler-node1
bb774b389f1b3       73deb9a3f7025       2 minutes ago       Running             etcd                      0                   6be9baaff6d50       etcd-node1
```

## 例子

- 查看所有镜像列表
```shell
sudo crictl images
```

- 查看镜像详情
```shell
sudo crictl inspecti 9cea3956c04b2
```

- 查看容器详情
```shell
sudo crictl inspect ubuntu
```
