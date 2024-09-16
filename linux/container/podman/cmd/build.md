# build

- 当前目录下有个Dockerfile文件，
使用podman build命令构建一个容器镜像。
```shell
podman build -t lsquic .
```

- 也可以指定Dockerfile文件路径
```shell
podman build -t lsquic -f Dockerfile
```

- url 方式构建
```shell
podman build https://192.168.0.12/podman-remote/Containerfile
```
