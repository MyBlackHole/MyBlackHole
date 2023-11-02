# rm


## 例子
- 删除目录
```shell
mc rm --recursive --force aio/dev-tools/rpc
mc ls aio/dev-tools/3.9.1.0/scripts/oracle/remote
```

- 删除本地文件，如下：
```shell
D:\\server\\minio>mc rm ping.txt
Removing \`ping.txt\`.
```

- 执行假删除操作，如下：
```shell
D:\\server\\minio>mc rm --fake ping.txt
Removing \`ping.txt\`.
```
执行完上面的 mc rm --fake 命令后，虽然提示删除成功；实际上 ping.txt 文件并没有被删除；  

- 从别名 Amazon S3 云存储中递归删除“jazz-songs”存储桶中前缀为 louis 的所有对象。
```shell
mc rm --recursive --force s3/jazz-songs/louis/
```

- 从别名 Amazon S3 云存储中递归删除“jazz-songs”存储桶中前缀为“louis”的所有早于 90 天的对象。
```shell
mc rm --recursive --force --older-than 90d s3/jazz-songs/louis/
```

- 从存储桶“pop-songs”中递归删除所有超过 7 天 10 小时的对象。  
```shell
mc rm --recursive --force --newer-than 7d10h s3/pop-songs/
```

- 删除从 STDIN 读取的所有对象。
```shell
mc rm --force --stdin
```

- 从 Amazon S3 云存储中递归删除所有对象。
```shell
mc rm --recursive --force --dangerous s3
```

- 递归删除所有存储桶下超过“90”天的所有对象。
```shell
mc rm --recursive --dangerous --force --older-than 90d s3
```

- 删除存储桶“jazz-songs”中所有不完整的上传。  
```shell
mc rm --incomplete --recursive --force s3/jazz-songs/
```

- 从 Amazon S3 云存储中删除加密对象。
```shell
mc rm --encrypt-key "s3/sql-backups/=32byteslongsecretkeymustbegiven1" s3/sql-backups/1999/old-backup.tgz
```

- 在治理模式下绕过对象保留并删除对象。
```shell
mc rm --bypass s3/pop-songs/
```

- 删除特定版本 ID。
```shell
mc rm s3/docs/money.xls --version-id "f20f3792-4bd4-4288-8d3c-b9d05b3b62f6"
```

- 删除所有超过一年的对象版本。
```shell
mc rm s3/docs/ --recursive --versions --rewind 365d
```
