# run

- 管道、重定向、输入输出重定向
```shell
run < /opt/aio/xtrabackup/backup_full.xbstream

如果带有参数
run -h 127.0.0.1 -p 6611 --nc 'xbstream -x -C /opt/aio/xtrabackup/xbstream_full_test1'  < /opt/aio/xtrabackup/backup_full.xbstream
```
