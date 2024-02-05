# lease

租约

## grant
```shell
etcdctl lease <subcommand> [flags]
```

## list
```shell
etcdctl lease list [flags]
```

## revoke
```shell
etcdctl lease revoke <leaseID> [flags]
```

## timetolive
```shell
etcdctl lease timetolive <leaseID> [options] [flags]
```

## keep-alive
```shell
etcdctl lease keep-alive [options] <leaseID> [flags]
```

## 例子
+ 添加租约
```shell
etcdctl lease grant 600
```

- 列出
```shell
etcdctl lease list
```

+ 删除
```shell
etcdctl lease revoke 694d81417acd4754
```

+ 详情
```shell
etcdctl lease timetolive 694d81417acd4759
```

+ 续约
```shell
etcdctl lease keep-alive 694d81417acd4757
```