# docker file

[细节](https://docs.docker.com/engine/reference/builder/)

有构建缓存机制
对每一个阶段命令生成
```shell
docker images --all
```

- 不使用缓存
```shell
--no-cache = true
```


|Dockerfile 指令|说明|
|---|---|
|FROM|指定基础镜像，用于后续的指令构建。|
|MAINTAINER|指定Dockerfile的作者/维护者。（已弃用，推荐使用LABEL指令）|
|LABEL|添加镜像的元数据，使用键值对的形式。|
|RUN|在构建过程中在镜像中执行命令。|
|CMD|指定容器创建时的默认命令。（可以被覆盖）|
|ENTRYPOINT|设置容器创建时的主要命令。（不可被覆盖）|
|EXPOSE|声明容器运行时监听的特定网络端口。|
|ENV|在容器内部设置环境变量。|
|ADD|将文件、目录或远程URL复制到镜像中。|
|COPY|将文件或目录复制到镜像中。|
|VOLUME|为容器创建挂载点或声明卷。|
|WORKDIR|设置后续指令的工作目录。|
|USER|指定后续指令的用户上下文。|
|ARG|定义在构建过程中传递给构建器的变量，可使用 "docker build" 命令设置。|
|ONBUILD|当该镜像被用作另一个构建过程的基础时，添加触发器。|
|STOPSIGNAL|设置发送给容器以退出的系统调用信号。|
|HEALTHCHECK|定义周期性检查容器健康状态的命令。|
|SHELL|覆盖Docker中默认的shell，用于RUN、CMD和ENTRYPOINT指令。|


## FROM
```Dockerfile
FROM [--platform=<platform>] <image> [AS <name>]
FROM [--platform=<platform>] <image>[:<tag>] [AS <name>]
FROM [--platform=<platform>] <image>[@<digest>] [AS <name>]
```

## COPY
- 拷贝使用指定阶段内容到当前镜像
```Dockerfile
FROM golang:1.20-alpine AS builder
...
RUN go build -ldflags="-s -w" -o /app/pub pub/pub.go

FROM alpine
COPY --from=builder /app/pub /app/pub
```
