# crictl
类似 docker


## config

```shell
<!-- sudo nvim /etc/crictl.yaml -->
# these first two endpoint setting is where you configure crictl to containerd
runtime-endpoint: unix:///run/containerd/containerd.sock
image-endpoint: unix:///run/containerd/containerd.sock
timeout: 2
debug: true
pull-image-on-create: false



```

## 例子
```shell
sudo crictl images
```
