# mysqlbinlog
解析 binlog 工具


## config
- 开启 binlog
```shell
<!-- 开启binlog日志 -->
log_bin=ON

<!-- 查询状态 -->
SHOW VARIABLES LIKE 'log_bin';

<!-- 查询 binlog 相关配置 -->
show global variables like "binlog%";
```

## 例子

- 解析 base64 处理过的 binlog
```shell
mysqlbinlog --base64-output=DECODE-ROWS -v binlog.000009 > binlog.sql

mysqlbinlog --no-defaults -v mysql-bin.000011 > binlog.sql

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

mysqlbinlog --no-defaults --database=aio --start-position=207195975 --stop-position=207208204 /opt/aio/mysql/binlog/mysql-bin.000050  > 50.dump.log

mysqlbinlog --no-defaults --stop-position=207195898 /opt/aio/mysql/binlog/*  > sql.log

```

- 获取远程
```shell
mysqlbinlog8.0 --no-defaults --read-from-remote-server --raw --host=192.168.79.25 --user=dbagent --password='Atf@2022' --result-file=/volmountpoint/aiopool/4089108a1417_20_44_1690442448_goldendb_log/binlog_1/ --port=5502 --to-last-log mysql-bin.001676
```
