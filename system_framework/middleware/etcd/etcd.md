# etcd

分布式键值数据库

[文档](https://etcd.io/docs/)
[cmd 命令](https://www.cnblogs.com/surpassme/p/16578301.html#:~:text=%23%20%E6%B7%BB%E5%8A%A0%E8%A7%92%E8%89%B2%20etcdctl%20role%20add%20etcd-test%20--user%3Droot%3Asurpass%20%23,--user%3Droot%3Asurpass%20%23%20%E5%88%A0%E9%99%A4%E8%A7%92%E8%89%B2%20etcdctl%20role%20delete%20etcd-test%20--user%3Droot%3Asurpass)

```shell
# 集群健康状态
etcdctl --endpoints=${ENDPOINTS} endpoint status -w table
etcdctl --endpoints=${ENDPOINTS} endpoint health -w table
```
## 例子
- docker 
```shell
<!-- 创建网络 -->
<!-- bridge 桥接 -->
<!-- app-tier 名 -->
docker network create app-tier --driver bridge

<!-- 启动 etcd 服务 -->
docker run -d --name etcd-server \
    --network app-tier \
    --publish 2379:2379 \
    --publish 2380:2380 \
    --env ALLOW_NONE_AUTHENTICATION=yes \
    --env ETCD_ADVERTISE_CLIENT_URLS=http://etcd-server:2379 \
    bitnami/etcd:latest

<!-- 测试 -->
docker run -it --rm \
    --network app-tier \
    --env ALLOW_NONE_AUTHENTICATION=yes \
    bitnami/etcd:latest etcdctl --endpoints http://etcd-server:2379 put /message Hello
etcd 06:07:22.86
etcd 06:07:22.86 Welcome to the Bitnami etcd container
etcd 06:07:22.86 Subscribe to project updates by watching https://github.com/bitnami/containers
etcd 06:07:22.86 Submit issues and feature requests at https://github.com/bitnami/containers/issues
etcd 06:07:22.87

OK


docker run -it --rm \
    --network app-tier \
    --env ALLOW_NONE_AUTHENTICATION=yes \
    bitnami/etcd:latest etcdctl --endpoints http://etcd-server:2379 get /message
etcd 06:07:31.93
etcd 06:07:31.93 Welcome to the Bitnami etcd container
etcd 06:07:31.93 Subscribe to project updates by watching https://github.com/bitnami/containers
etcd 06:07:31.93 Submit issues and feature requests at https://github.com/bitnami/containers/issues
etcd 06:07:31.94

/message
Hello
```
