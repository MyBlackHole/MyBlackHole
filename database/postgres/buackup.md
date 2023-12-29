# buackup

- 获取系统标识与时间线
```shell
❯ psql "dbname=postgres replication=database" -c "IDENTIFY_SYSTEM"
      systemid       | timeline |  xlogpos  |  dbname
---------------------+----------+-----------+----------
 7316146825276107017 |        1 | 0/1544A80 | postgres
(1 row)
```

- 获取时间线历史
```shell
psql "dbname=postgres replication=database" -c "TIMELINE_HISTORY 1"
```
