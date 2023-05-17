# gs_probackup
备份还原工具

## 配置 
1. 备份实例目录下的pg_probackup.conf文件(指定备份时使用的相关参数)
2. 备份集目录下的backup.control文件(描述备份集的属性信息)

## 命令

- add-instance 添加实例
```shell
gs_probackup add-instance -B backup-path                       ## 备份路径
                          -D pgdata-path                       ## 数据目录路径
                          --instance=instance_name             ## 实例名(自定义)
                          [-E external-directories-paths]      ## 需要备份的其他目录
                          [--remote-proto=protocol]            ## 远程操作的协议(仅支持SSH)
                          [--remote-host=destination]          ## 远程主机IP或主机名
                          [--remote-path=path]                 ## 远程主机的gs_probackup安装目录
                          [--remote-user=username]             ## 远程主机SSH连接的用户
                          [--remote-port=port]                 ## 远程主机SSH连接的端口(默认22)
                          [--ssh-options=ssh_options]          ## SSH命令行参数
                          [--help]                             ## 显示帮助信

# 例如
gs_probackup add-instance -B /home/omm/backup_1 -D /var/lib/opengauss/data --instance gs_master_backup_1
```

- show
```shell
gs_probackup show -B backup-path
                 [--instance=instance_name [-i backup-id]]
                 [--archive] [--format=format]

  -B, --backup-path=backup-path    location of the backup storage area
      --instance=instance_name     show info about specific instance
  -i, --backup-id=backup-id        show info about specific backups
      --archive                    show WAL archive information
      --format=format              show format=PLAIN|JSON

gs_probackup show -B /home/omm/backup_1/
gs_probackup show -B /home/omm/backup_1/ --instance=gs_master_backup_1 -i RUQME4
```

- backup 备份
```shell
注意：建议加上 -d 指定备份时连接的数据库，否则默认会使用和当前用户同名的数据库，这样会因为这个数据库不存在而导致报错
gs_probackup backup -B backup-path --instance=instance_name [-D pgdata-path]
    -b backup-mode                                       ## 备份模式：FULL(全量备份)/PTRACK(增量备份)
    [-C]                                                 ## -C指定检查点模式为spread(周期调度)，默认为fast快速完成
    [-S slot-name] [--temp-slot]                         ## -S指定物理复制slot名称(默认pg_probackup_slot) ,--temp-slot为WAL流处理创建临时物理复制slot，确保备份过程中所需的WAL段仍然可用
    [--backup-pg-log]                                    ## 备份日志目录(默认不备份)
    [-j threads_num]                                     ## 并行线程数
    [--progress]                                         ## 显示进度
    [--no-validate]                                      ## 完成备份后跳过自动验证
    [--skip-block-validation]                            ## 关闭块级校验，加快备份速度
    [-E external-directories-paths]                      ## 需要备份的其他目录
    [--no-sync]                                          ## 不将备份文件同步写入磁盘，提升写入性能
    [--note=text]                                        ## 备份的注释
    [--archive-timeout=timeout]                          ## 以秒为单位设置流式处理的超时时间(默认300)
    [--log-level-console=log-level-console]              ## 设置要发送到控制台的日志级别，默认为info，设置off可以关闭
    [--log-level-file=log-level-file]                    ## 设置要发送到日志文件的日志级别，默认off
    [--log-filename=log-filename]                        ## 指定要创建的日志文件的文件名(strftime模式示例：pg_probackup-%u.log),默认pg_probackup.log
    [--error-log-filename=error-log-filename]            ## error日志的日志文件名
    [--log-directory=log-directory]                      ## gs_probackup的日志目录
    [--log-rotation-size=log-rotation-size]              ## gs_probackup的日志大小(默认单位KB,支持KB、MB、GB、TB)，超过阈值后进行循环，默认0(禁用)
    [--log-rotation-age=log-rotation-age]                ## 单个日志文件的最大生命周期(默认单位min,支持ms, s, min, h, d)，默认0表示禁用基于时间的循环
    [--delete-expired]                                   ## 删除不符合pg_probackup.conf配置文件中定义的留存策略的备份
    [--delete-wal]                                       ## 删除不需要的WAL文件
    [--merge-expired]                                    ## 将满足留存策略要求的最旧的增量备份与其已过期的父备份合并
    [--retention-redundancy=retention-redundancy]        ## 留存的完整备份数(默认0,禁用此设置)
    [--retention-window=retention-window]                ## 留存的天数(默认0,禁用此设置)
    [--wal-depth=wal-depth]                              ## 每个时间轴上必须留存的执行PITR能力的最新有效备份数(默认0,禁用此设置)
    [--dry-run]                                          ## 显示所有可用备份的当前状态，不删除或合并过期备份
    [--ttl=interval]                                     ## 从恢复时间开始计算，备份要固定的时间量(支持单位：ms, s, min, h, d(默认为s))
    [--expire-time=time]                                 ## 备份固定失效的时间戳(例如：--expire-time='2020-01-01 00:00:00+03')
    [--compress-algorithm=compress-algorithm][--compress-level=compress-level][--compress]   ## 压缩参数：压缩算法(zlib/pglz/none)/压缩级别(0~9,默认1)/压缩(相当于zlib算法+1压缩级别) 
    [-d dbname] [-h host] [-p port] [-U username] [-w] [-W password]                         ## 连接信息：数据库名/主机名/端口/用户/密码
    [--remote-proto=protocol] [--remote-host=destination]
    [--remote-path=path] [--remote-user=username]
    [--remote-port=port] [--ssh-options=ssh_options]
    [--help]

# 例如 全量备份
gs_probackup backup -B /home/omm/backup_1 --instance gs_master_backup_1 -b full -D /var/lib/opengauss/data --progress \
    --log-directory=/home/omm/backup_1/log  --log-rotation-size=10GB --log-rotation-age=30d  --log-level-file=info  --log-filename=full_20230516.log \
    --retention-redundancy=2 \
    --compress  \
    --note='full backup test 1'

# 例如 增量备份

gs_probackup backup -B /home/omm/backup_1 --instance gs_master_backup_1 -b PTRACK -D /var/lib/opengauss/data --progress \
    --log-directory=/home/omm/backup_1/log  --log-rotation-size=10GB --log-rotation-age=30d  --log-level-file=info  --log-filename=incr1_20230516.log \
    --delete-expired --delete-wal \
    --retention-redundancy=2 \
    --compress  \
    --note='incremental backup test 1'
```

- restore 恢复 
```shell
gs_probackup restore -B backup-path --instance=instance_name
                 [-D pgdata-path] [-i backup-id] [-j threads_num] [--progress]
                 [--force]                                 ## 允许从损坏的或无效的备份中恢复数据(慎用)
                 [--no-sync]                               ## 不同步写入磁盘,以提升恢复效率
                 [--no-validate] [--skip-block-validation] ## 跳过备份验证/跳过块级验证(仅做文件级别验证)
                 [--external-mapping=OLDDIR=NEWDIR]        ## 恢复时重新指定目录OLDIR为NEWDIR
                 [-T OLDDIR=NEWDIR]                        ## 与--external-mapping配合使用,重新指定表空间路径(将表空间路径OLDDIR改为NEWDIR)
                 [--skip-external-dirs]                    ## 不恢复备份文件中--external-dirs选项所指定的外部目录
                 [-I incremental_mode]                     ## 增量模式none|checksum|lsn,默认none
                 [--recovery-target-time=time              ## 指定恢复的目标time(只能指定备份信息内的recovery-time) , 备份信息使用gs_probackup show查看
                 |--recovery-target-xid=xid                ## 指定恢复的目标xid(只能指定备份信息内的recovery-xid)
                 |--recovery-target-lsn=lsn                ## 指定恢复的目标lsn(只能指定备份信息内的stop-lsn)
                 |--recovery-target-name=target-name]      ## 指定恢复的目标保存点(只能指定备份信息内的recovery-name)
                 [--recovery-target-inclusive=boolean]     ## true(恢复目标将包括指定的内容) | false(恢复目标将不包括指定的内容),与以上recovery-target配合使用
                 [--remote-proto=protocol] [--remote-host=destination][--remote-path=path] [--remote-user=username][--remote-port=port] [--ssh-options=ssh_options] ## 远程连接信息
                 [--log-level-console=log-level-console]
                 [--log-level-file=log-level-file]
                 [--log-filename=log-filename]
                 [--error-log-filename=error-log-filename]
                 [--log-directory=log-directory]
                 [--log-rotation-size=log-rotation-size]
                 [--log-rotation-age=log-rotation-age]
                 [--help]
## 其他参数介绍参考前面的 gs_probackup backup语法介绍.

# 例如
gs_probackup restore -B /home/omm/backup_1/ --instance=gs_master_backup_1 -D /var/lib/opengauss/data -i RUQME4 --progress -j 4    ##  -i指定备份文件ID，RUQME4即为全备的ID
```

- del-instance 删除备份实例
```shell
gs_probackup del-instance -B /home/omm/backup_1/ -D /var/lib/opengauss/data --instance=gs_master_backup_1
```

- delete 删除备份集
```shell
gs_probackup delete -B /home/omm/backup_1/ --instance=gs_master_backup_1 -i RUQME4
```

- merge 合并备份集
```shell
# 合并后 RUQNFT 前的所有备份集删除 RUQNFT 变为全量 (本质是合并到全量然后重命名为 RUQNFT)
gs_probackup merge -B /home/omm/backup_1/ --instance=gs_master_backup_1 -i RUQNFT --progress
```

- validate 验证备份集
```shell
gs_probackup validate -B /home/omm/backup_1/ --instance=gs_master_backup_1 -i RUQNFT
```
