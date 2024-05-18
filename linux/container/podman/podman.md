# podman

## 安装
- centos7
```shell
非常麻烦
```
- ubuntu
```shell
sudo apt-get update
sudo apt-get install -y podman
```

## 基本用法
- 启动 podman 服务
```shell
sudo podman system service -t 0 &
```

- 运行容器
```shell
podman run --name my-container -d nginx
```

- 停止容器
```shell
podman stop my-container
```

## 常用命令
- 列出所有容器
```shell
podman ps -a
```

- 进入容器
```shell
podman exec -it my-container bash
```

## 配置镜像源
- 配置镜像源
```shell
sudo lvim /etc/containers/registries.conf
unqualified-search-registries = ["docker.io"]
```


## 开发环境

```shell
<!-- ubuntu -->
podman run --name dev -v /run/media/black/Data:/run/media/black/Data/ -it ubuntu:20.04 bash
apt update
apt-get install git wget rpm rpm2cpio cpio make build-essential binutils libtinfo5

<!-- centos -->
podman run --name dev -v /run/media/black/Data:/run/media/black/Data/  -it centos:centos7 bash
yum install -y git wget rpm* cpio make glibc-devel glibc-headers binutils m4
```
