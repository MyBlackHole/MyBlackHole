# pg_probackup

## build
```shell
make USE_PGXS=1 PG_CONFIG=/run/media/black/Data/lib/postgres/zh_master/bin/pg_config top_srcdir=/run/media/black/Data/Documents/github/C/postgres
```

## init

```shell
./pg_probackup init \
    -B `pwd`/Debug/backup


tree Debug
Debug
└── backup
    ├── backups
    └── wal

4 directories, 0 files
```

## add instance
```shell

./pg_probackup add-instance \
    -B `pwd`/Debug/backup \
    -D /run/media/black/Data/Documents/github/C/postgres/Debug/database/ \
    --instance=local
INFO: Instance 'local' successfully initialized


tree Debug
Debug
└── backup
    ├── backups
    │   └── local
    │       └── pg_probackup.conf
    └── wal
        └── local

6 directories, 1 file


./pg_probackup add-instance \
    -B `pwd`/Debug/backup \
    -D /run/media/black/Data/Documents/github/C/postgres/Debug/database/ \
    --instance=local \
    --remote-host=127.0.0.1 \
    --remote-port=22 \
    --remote-user=root \
    --remote-path=`pwd`
```

## backup

- full
```shell
<!-- 备份本地实例 -->
./pg_probackup backup \
    -B `pwd`/Debug/backup \
    --instance local \
    --stream \
    -b full \
    -d postgres

INFO: Backup start, pg_probackup version: 2.5.13, instance: local, backup ID: SB7IQQ, backup mode: FULL, wal mode: STREAM, remote: false, compress-algorithm: none, compress-level: 1
WARNING: This PostgreSQL instance was initialized without data block checksums. pg_probackup have no way to detect data block corruption without them. Reinitialize PGDATA with option '--data-checksums'.
WARNING: Current PostgreSQL role is superuser. It is not recommended to run pg_probackup under superuser.
INFO: Database backup start
INFO: wait for pg_backup_start()
INFO: Wait for WAL segment /run/media/black/Data/Documents/github/C/pg_probackup/Debug/backup/backups/local/SB7IQQ/database/pg_wal/0000000100000000000000C0 to be streamed
INFO: PGDATA size: 60MB
INFO: Current Start LSN: 0/C0000028, TLI: 1
INFO: Start transferring data files
INFO: Data files are transferred, time elapsed: 1s
INFO: wait for pg_stop_backup()
INFO: pg_stop backup() successfully executed
INFO: stop_lsn: 0/C0000188
INFO: Getting the Recovery Time from WAL
INFO: Syncing backup files to disk
INFO: Backup files are synced, time elapsed: 0
INFO: Validating backup SB7IQQ
INFO: Backup SB7IQQ data files are valid
INFO: Backup SB7IQQ resident size: 92MB
INFO: Backup SB7IQQ completed


 
<!-- 备份远程实例 -->
./pg_probackup backup \
    -B `pwd`/Debug/backup \
    --instance local \
    -b full \
    -d postgres
 
<!-- 如果需包含外部目录 -->
--external-dirs=/etc/dir1:/etc/dir2
```

- archive (?)
```shell
./pg_probackup archive-get \
    -B `pwd`/Debug/backup \
    --instance local \
    -d postgres
```

- inc
```shell

<!-- delta -->
./pg_probackup backup \
    -B `pwd`/Debug/backup \
    --instance local \
    -d postgres \
    --stream \
    -b delta

<!-- page -->
./pg_probackup backup \
    -B `pwd`/Debug/backup \
    --instance local \
    --stream \
    -d postgres \
    -b page

INFO: Backup start, pg_probackup version: 2.5.13, instance: local, backup ID: SB7IXK, backup mode: PAGE, wal mode: STREAM, remote: false, compress-algorithm: none, compress-level: 1
WARNING: This PostgreSQL instance was initialized without data block checksums. pg_probackup have no way to detect data block corruption without them. Reinitialize PGDATA with option '--data-checksums'.
WARNING: Current PostgreSQL role is superuser. It is not recommended to run pg_probackup under superuser.
INFO: Database backup start
INFO: wait for pg_backup_start()
INFO: Parent backup: SB7IQQ
INFO: Wait for WAL segment /run/media/black/Data/Documents/github/C/pg_probackup/Debug/backup/wal/local/0000000100000000000000CC to be archived
INFO: Wait for WAL segment /run/media/black/Data/Documents/github/C/pg_probackup/Debug/backup/backups/local/SB7IXK/database/pg_wal/0000000100000000000000CC to be streamed
INFO: PGDATA size: 180MB
INFO: Current Start LSN: 0/CC000028, TLI: 1
INFO: Parent Start LSN: 0/C0000028, TLI: 1
INFO: Extracting pagemap of changed blocks
INFO: Pagemap successfully extracted, time elapsed: 0 sec
INFO: Start transferring data files
INFO: Data files are transferred, time elapsed: 1s
INFO: wait for pg_stop_backup()
INFO: pg_stop backup() successfully executed
INFO: stop_lsn: 0/CD0000F0
INFO: Getting the Recovery Time from WAL
INFO: Syncing backup files to disk
INFO: Backup files are synced, time elapsed: 0
INFO: Validating backup SB7IXK
INFO: Backup SB7IXK data files are valid
INFO: Backup SB7IXK resident size: 199MB
INFO: Backup SB7IXK completed


 
<!-- ptrack -->
./pg_probackup backup \
    -B `pwd`/Debug/backup \
    --instance local \
    -d postgres \
    -b ptrack
```

## merge
```shell
./pg_probackup merge \
    -B `pwd`/Debug/backup \
    --instance local \
    -i SB7IXK

INFO: Merge started
INFO: Merging backup SB7IXK with parent chain
INFO: Validate parent chain for backup SB7IXK
INFO: Validating backup SB7IQQ
INFO: Backup SB7IQQ data files are valid
INFO: Validating backup SB7IXK
INFO: Backup SB7IXK data files are valid
INFO: Start merging backup files
INFO: Backup files are successfully merged, time elapsed: 1s
INFO: Delete: SB7IXK 2024-03-31 18:11:00+08
INFO: Rename merged full backup SB7IQQ to SB7IXK
INFO: Validating backup SB7IQQ
INFO: Backup SB7IQQ data files are valid
INFO: Merge of backup SB7IXK completed
```


## show 

- backup info
```shell
./pg_probackup show \
    -B `pwd`/Debug/backup

BACKUP INSTANCE 'local'
===================================================================================================================================
 Instance  Version  ID      Recovery Time           Mode  WAL Mode  TLI  Time   Data   WAL  Zratio  Start LSN   Stop LSN    Status
===================================================================================================================================
 local     17       SB7IXK  2024-03-31 18:11:00+08  PAGE  STREAM    1/1   35s  151MB  48MB    1.00  0/CC000028  0/CD0000F0  OK
 local     17       SB7IQQ  2024-03-31 18:06:28+08  FULL  STREAM    1/0   10s   60MB  32MB    1.00  0/C0000028  0/C0000188  OK
```

- archive info
```shell
./pg_probackup show \
    -B `pwd`/Debug/backup \
    --instance local \
    --archive
```


## 保留策略 (retention policy)
```shell
./pg_probackup set-config \
    -B `pwd`/Debug/backup \
    --instance local \
    --retention-redundancy=20
 
./pg_probackup set-config \
    -B `pwd`/Debug/backup \
    --instance local \
    --retention-window=7
```


## restore
```shell
<!-- 223 back host操作 -->
./pg_probackup restore  \
    -B `pwd`/Debug/backup \
    -D /run/media/black/Data/Documents/github/C/postgres/Debug/database/ \
    --instance pg200_5432 \
    --remote-user=postgres \
    --remote-host=192.168.99.200 \
    --remote-port=22 \
    --archive-host=192.168.99.223 \
    --archive-port=22 \
    --archive-user=postgres
 
<!-- 恢复之后需要重做基础备份，后续才能继续做增量备份 -->
./pg_probackup backup \
    -B `pwd`/Debug/backup \
    --instance pg200_5432 \
    -d postgres \
    -b full
 
<!-- 再次恢复 -->
./pg_probackup restore  \
    -B `pwd`/Debug/backup \
    -D /run/media/black/Data/Documents/github/C/postgres/Debug/database/ \
    --instance pg200_5432 \
    --remote-user=postgres \
    --remote-host=192.168.99.200 \
    --remote-port=22 \
    --archive-host=192.168.99.223 \
    --archive-port=22 \
    --archive-user=postgres
```
