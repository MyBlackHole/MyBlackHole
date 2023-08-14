# list
查询展示
-o: 指定返回字段
-t: 指定查询类型

- 查询
```shell
zfs list -t snapshot -o name,volsize,used,refer,avail
```