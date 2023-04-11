# ldb
rocksdb 工具

## 例子
- 查询所有数据
```shell
./ldb --db=/tmp/rocksdb_simple scan
```

- 解析 wal log 
```shell
./ldb dump_wal --walfile=/tmp/rocksdb_simple/000019.log --header
```
