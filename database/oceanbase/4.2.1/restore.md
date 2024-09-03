# restore


```shell
ALTER SYSTEM RESTORE mysql_test FROM 'oss://backup/data?host=10.5.13.27&access_id=1358&access_key=1358,oss://backup/archive/?host=10.6.66.9&access_id=1358&access_key=1358' UNTIL TIME='2024-09-03 03:32:11' WITH 'pool_list=restore_pool';
```
