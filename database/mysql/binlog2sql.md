# binlog2sql

数据恢复命令

前提: (混杂模式有部分操作缺失)
binlog_format: ROW

```shell
<!-- 输出正向 sql -->
python3 binlog2sql/binlog2sql.py -h192.168.60.70 -P3306 -uroot -p'p@3Sw0rd' --stop-position=207195898 --start-file='mysql-bin.000048'

<!-- -B 输出回滚 sql -->
python3 binlog2sql/binlog2sql.py -h192.168.60.70 -P3306 -uroot -p'p@3Sw0rd' --stop-position=207195898 --start-file='mysql-bin.000048' -B
```
