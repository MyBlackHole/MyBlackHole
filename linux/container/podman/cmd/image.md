# image

镜像管理相关命令。

- 导出镜像
```shell
podman image save docker.io/sophgo/tpuc_dev:v3.1 -o tpuc_dev.v3_1.tar
```

- 导入镜像
```shell
podman image load -i tpuc_dev.v3_1.tar
```
- 列出镜像
```shell
podman image ls
```

- 删除镜像
```shell
podman image rm <image_name>
```

- 镜像历史
```shell
podman image history <image_name>
```

- 镜像标签
```shell
podman image tag <image_name> <new_image_name>
```

- 镜像推送
```shell
podman image push <image_name>
```

- 镜像拉取
```shell
podman image pull <image_name>
```
