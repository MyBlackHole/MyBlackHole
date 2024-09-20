# restore


```shell
# 恢复数据
ALTER SYSTEM RESTORE mysql_test FROM 'oss://backup/data_4216?host=10.5.13.27&access_id=1358&access_key=1358,oss://backup/archive_4216/?host=10.5.13.27&access_id=1358&access_key=1358' UNTIL TIME='2024-09-20 10:48:10' WITH 'pool_list=mq_pool_restore';

# 查看 restore 进度
SELECT * FROM oceanbase.CDB_OB_RESTORE_PROGRESS\G

# 查看 restore 历史
SELECT * FROM oceanbase.CDB_OB_RESTORE_HISTORY order by job_id desc \G
```
