# ctr

## images

- 指定命名空间
```shell
sudo ctr -n=k8s.io i ls
```

### export
- 导出镜像
```shell
ctr images export pause.img registry.k8s.io/pause:3.9
```

### import
- 导入镜像
```shell
ctr image import image.img

#使用ctr命令指定命名空间导入镜像
ctr -n=k8s.io image import dashboard.tar

#查看镜像，可以看到可以查询到了
crictl images
```

### rm 

```shell
ctr images rm registry.k8s.io/coredns/coredns:v1.10.1
```

### namespaces

```shell
sudo ctr namespaces list
```
