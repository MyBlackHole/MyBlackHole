# debug 

## task
 
```shell
export $(xargs < /opt/aio/cfg/airflow.runtime.env)
source /opt/aio/airflow/bin/activate

<!-- interactive 交互 log 模式 -->
<!-- subdir 指定 dag 目录 -->
<!-- raw 原始运行 task -->
<!-- local 运行 job 与 task  -->
<!-- aio_full_backup(dag 名) -->
<!-- zfs到zfs(task 名) -->
<!-- manual__2023-09-04T02:32:50.656654+00:00(批次) -->
airflow tasks run aio_full_backup zfs到zfs manual__2023-09-04T02:32:50.656654+00:00 --job-id 148121 --interactive --raw --subdir DAGS_FOLDER/aio_full_backup.py
airflow tasks run goldendb_gtmlog_backup 获取活跃事务列表备份起始位置以及大小 manual__2023-09-12T09:27:35.458090+00:00 --interactive --raw --subdir DAGS_FOLDER/goldendb_gtmlog_backup.py

ActP@ssw0rd
```
