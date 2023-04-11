# sst_dump


## 例子
- 读取 sst
```shell
./sst_dump --file=/tmp/rocksdb_c_simple_example/000013.sst --command=scan 
```

- 打印 sst file 配置
```shell
./sst_dump --file=/path/to/sst/000829.sst --show_properties
```
