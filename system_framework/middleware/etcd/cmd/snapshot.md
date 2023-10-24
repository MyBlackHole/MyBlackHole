# snapshot
备份和恢复

```shell
etcdctl snapshot <subcommand> [flags]
```

## save
```shell
etcdctl snapshot save <filename> [flags]
```

## status
```shell
etcdctl snapshot status <filename> [flags]
```

## restore
```shell
etcdctl snapshot restore <filename> [options] [flags]
```

## 例子
+ 生成快照
```shell
etcdctl snapshot save Surpass.db
```

+ 查看快照
```shell
etcdctl snapshot status Surpass.db -w table
```

+ 恢复快照
```shell
etcdctl snapshot restore Surpass.db \
--name=default \
--data-dir=/home/etcd_db \
--initial-advertise-peer-urls=http://localhost:2380
```