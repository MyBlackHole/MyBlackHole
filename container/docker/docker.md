# docker


### 参数

- --link
```shell
--link可以通过容器名互相通信，容器间共享环境变量。
--link主要用来解决两个容器通过ip地址连接时容器ip地址会变的问题.
```

## 例子
- 删除所有 dangling (悬挂) 数据卷
```shell
docker volume rm $(docker volume ls -qf dangling=true)
```

- 删除所有关闭的容器
```shell
docker ps -a | grep Exit | cut -d ’ ’ -f 1 | xargs docker rm
```

- 删除关闭的容器、无用的数据卷和网络
```shell
docker system prune
```

- 删除更彻底，可以将没有容器使用Docker镜像都删掉
```shell
docker system prune -a
```
