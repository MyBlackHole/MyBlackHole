# del

```shell
etcdctl del [options] <key> [range_end] [flags]
```

|参数|功能描述|
|---|---|
|--prefix|根据prefix进行匹配删除|
|--prev-kv|输出删除的键值|
|--from-key|按byte进行比较，删除大于等于指定key的结果|
