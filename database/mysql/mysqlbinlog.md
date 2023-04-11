# mysqlbinlog
解析 binlog 工具

## 例子

- 解析 base64 处理过的 binlog
```shell
mysqlbinlog --base64-output=DECODE-ROWS -v binlog.000009 > binlog.sql

```

- 解析指定时间范围
```shell
mysqlbinlog --start-datetime=‘2020-06-12 00:00:00’ \
            --stop-datetime=‘2020-06-12 17:54:49’ \
            --base64-output=DECODE-ROWS -v binlog.000009 > binlog.sql
```

- 指定 pos 范围值查看指定数据库
```shell
mysqlbinlog --start-position=2098 \
            --stop-position=2205 \
            -d user \
            --base64-output=DECODE-ROWS -v binlog.000009 > binlog.sql
```
