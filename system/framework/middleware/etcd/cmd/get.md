# get

```shell
etcdctl get [options] <key> [range_end] [flags]
```

|参数|功能描述|
|---|---|
|--hex|以十六进制形式输出|
|--limit number|设置输出结果的最大值|
|--prefix|根据prefix进行匹配key|
|--order|对输出结果进行排序，ASCEND 或 DESCEND|
|--sort-by|按给定字段排序，CREATE, KEY, MODIFY, VALUE, VERSION|
|--print-value-only|仅输出value值|
|--from-key|按byte进行比较，获取大于等于指定key的结果|
|--keys-only|仅获取keys|

## 例子

- 查询前缀所有 key
```shell
etcdctl get --prefix ""
```
