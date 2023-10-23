# watch

监听

```shell
etcdctl watch [options] [key or prefix] [range_end] [--] [exec-command arg1 arg2 ...] [flags]
```

## 例子
```shell
# 对某个key监听操作，当key1发生改变时，会返回最新值
etcdctl watch name
# 监听key前缀
etcdctl watch name --prefix
# 监听到改变后执行相关操作
etcdctl watch name --  etcdctl get age
```