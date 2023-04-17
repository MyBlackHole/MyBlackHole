# 异常处理集

- 手动备份异常
```shell
# log 执行内容
2023-04-12 14:31:56.241 INFO   [https-jsse-nio-9093-exec-9] c.t.t.s.B.i.BackupRecoveryServiceImpl - create national manual backup task cmd : python /teledb/telemonitor-stg/telemonitor/python/operate/create_manual_backup_job_in_national.py -c 1 -r 1
# 异常报错信息
this db instance is not suitable for manual backup (数据库实例不适合手动备份)

# 查看
create_manual_backup_job_in_national.py
```
