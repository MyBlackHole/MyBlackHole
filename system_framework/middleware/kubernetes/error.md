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
