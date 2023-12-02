# update


# Docker 容器的重启策略如下：
 --restart具体参数值详细信息：
       no　　　　　　　 // 默认策略,容器退出时不重启容器；
       on-failure　　  // 在容器非正常退出时（退出状态非0）才重新启动容器；
       on-failure:3    // 在容器非正常退出时重启容器，最多重启3次；
       always　　　　  // 无论退出状态是如何，都重启容器；
       unless-stopped  // 在容器退出时总是重启容器，但是不考虑在 Docker 守护进程启动时就已经停止了的容器。


- 容器自动重启
```shell
docker run -d --restart=always --name 设置容器名 使用的镜像
（上面命令  --name后面两个参数根据实际情况自行修改）
 
<!-- 已有 -->
docker update --restart=always 容器ID(或者容器名)
（容器ID或者容器名根据实际情况修改）
```
