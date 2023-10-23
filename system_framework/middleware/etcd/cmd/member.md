# member

## update
```shell
etcdctl member update <memberID> [options] [flags]
```

## remove
```shell
etcdctl member remove <memberID> [flags]
```


## 例子
+ 删除成员
```shell
etcdctl member remove ade526d28b1f92f7
```

+ 更新成员
```shell
etcdctl member update ade526d28b1f92f7 --peer-urls=http://192.168.3.111:2380
```

+ 添加成员
```shell
# 示例：将目标节点etcd4添加到集群
etcdctl member add etcd4 http://192.168.3.104:2380
# 启动目标集群时需要设置启动参数如下
etcd --name=etcd4 --data-dir=/etcd-data \
  --listen-peer-urls=http://192.168.3.104:2380 \
  --listen-client-urls=http://192.168.3.104:2379 \
  --initial-advertise-peer-urls=http://192.168.3.104:2380 \
  --advertise-client-urls=http://192.168.3.104:2379 \
  --initial-cluster=etcd1=http://192.168.3.101:2380,etcd2=http://192.168.3.102:2380,etcd3=http://192.168.3.103:2380,etcd4=http://192.168.3.104:2380 \
  --initial-cluster-state=existing
```

