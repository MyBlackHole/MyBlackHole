# podman


- 开发环境
```shell
<!-- ubuntu -->
podman run --name dev -v /run/media/black/Data:/run/media/black/Data/ -it ubuntu:20.04 bash
apt update
apt-get install git wget rpm rpm2cpio cpio make build-essential binutils libtinfo5

<!-- centos -->
podman run --name dev -v /run/media/black/Data:/run/media/black/Data/  -it centos:centos7 bash
yum install -y git wget rpm* cpio make glibc-devel glibc-headers binutils m4
```
