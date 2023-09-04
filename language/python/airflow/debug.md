# debug 

## task
 
```shell
<!-- interactive 交互 log 模式 -->
<!-- subdir 指定 dag 目录 -->
<!-- raw 原始运行 task -->
<!-- local 运行 job 与 task  -->
<!-- aio_full_backup(dag 名) -->
<!-- zfs到zfs(task 名) -->
<!-- manual__2023-09-04T02:32:50.656654+00:00(批次) -->
airflow tasks run aio_full_backup zfs到zfs manual__2023-09-04T02:32:50.656654+00:00 --job-id 148121 --interactive --raw --subdir DAGS_FOLDER/aio_full_backup.py
```
