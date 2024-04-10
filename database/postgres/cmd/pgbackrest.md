# pgbackrest

- config
```shell
<!-- lvim pgbackrest.conf -->
[pg01]
pg01-path=/var/lib/postgres/data/
pg01-port=5432
pg01-user=postgres
pg01-socket-path=/run/postgresql

[global]
repo1-path=/data/pgbackup
retention-full=2
repo1-cipher-pass=3BpN1NVJOSRNhajKAk7Fs+PE4vZeQNK/+aENDuUz4xyOsPxXW6Ug7qolr7m+vBcb
repo1-cipher-type=aes-256-cbc
start-fast=y
process-max=3
log-path=/data/log

[global:archive-push]
compress-level=3


sudo lvim /var/lib/postgres/data/postgresql.conf
archive_mode = on
archive_command = 'pgbackrest --config /data/pgbackrest.conf --stanza=pg01 archive-push %p'↴
```

- init
```shell
sudo chown -R postgres:postgres /data

sudo su postgres

# 初始化
pgbackrest \
    --config /data/pgbackrest.conf \
    --stanza=pg01 \
    --log-level-console=info \
    stanza-create

2024-04-01 18:10:54.673 P00   INFO: stanza-create command begin 2.52dev: --config=/data/pgbackrest.conf --exec-id=221150-2f7dbdfa --log-level-console=info --log-path=/data/log --pg1-path=/var/lib/postgres/data/ --pg1-port=5432 --pg1-socket-path=/run/postgresql --pg1-user=postgres --repo1-cipher-pass=<redacted> --repo1-cipher-type=aes-256-cbc --repo1-path=/data/pgbackup --stanza=pg01
ERROR: [050]: unable to acquire lock on file '/tmp/pgbackrest/pg01-archive.lock': Permission denied
       HINT: does 'postgres:postgres' running pgBackRest have permissions on the '/tmp/pgbackrest/pg01-archive.lock' file?
2024-04-01 18:10:54.674 P00   INFO: stanza-create command end: aborted with exception [050]
[postgres@black ~]$ pgbackrest     --config /data/pgbackrest.conf     --stanza=pg01     --log-level-console=info     stanza-create
2024-04-01 18:11:17.635 P00   INFO: stanza-create command begin 2.52dev: --config=/data/pgbackrest.conf --exec-id=221396-cad1c46d --log-level-console=info --log-path=/data/log --pg1-path=/var/lib/postgres/data/ --pg1-port=5432 --pg1-socket-path=/run/postgresql --pg1-user=postgres --repo1-cipher-pass=<redacted> --repo1-cipher-type=aes-256-cbc --repo1-path=/data/pgbackup --stanza=pg01
2024-04-01 18:11:18.242 P00   INFO: stanza-create for stanza 'pg01' on repo1
2024-04-01 18:11:18.253 P00   INFO: stanza-create command end: completed successfully (623ms)
```

- 全量备份
```shell
<!-- --compress false \ -->
pgbackrest \
    --config /data/pgbackrest.conf \
    --stanza=pg01 \
    --log-level-console=info \
    --type=full \
    backup

2024-04-01 18:15:05.536 P00   INFO: backup command begin 2.52dev: --config=/data/pgbackrest.conf --exec-id=222126-08cc226a --log-level-console=info --log-path=/data/log --pg1-path=/var/lib/postgres/data/ --pg1-port=5432 --pg1-socket-path=/run/postgresql --pg1-user=postgres --process-max=3 --repo1-cipher-pass=<redacted> --repo1-cipher-type=aes-256-cbc --repo1-path=/data/pgbackup --repo1-retention-full=2 --stanza=pg01 --start-fast
WARN: no prior backup exists, incr backup has been changed to full
2024-04-01 18:15:06.246 P00   INFO: execute non-exclusive backup start: backup begins after the requested immediate checkpoint completes
2024-04-01 18:15:06.949 P00   INFO: backup start archive = 000000010000000000000007, lsn = 0/7000028
2024-04-01 18:15:06.949 P00   INFO: check archive for prior segment 000000010000000000000006
2024-04-01 18:15:10.401 P00   INFO: execute non-exclusive backup stop and wait for all WAL segments to archive
2024-04-01 18:15:10.602 P00   INFO: backup stop archive = 000000010000000000000007, lsn = 0/7000138
2024-04-01 18:15:10.607 P00   INFO: check archive for segment(s) 000000010000000000000007:000000010000000000000007
2024-04-01 18:15:10.625 P00   INFO: new backup label = 20240401-181506F
2024-04-01 18:15:10.679 P00   INFO: full backup size = 22.2MB, file total = 966
2024-04-01 18:15:10.679 P00   INFO: backup command end: completed successfully (5148ms)
2024-04-01 18:15:10.679 P00   INFO: expire command begin 2.52dev: --config=/data/pgbackrest.conf --exec-id=222126-08cc226a --log-level-console=info --log-path=/data/log --repo1-cipher-pass=<redacted> --repo1-cipher-type=aes-256-cbc --repo1-path=/data/pgbackup --repo1-retention-full=2 --stanza=pg01
2024-04-01 18:15:10.686 P00   INFO: expire command end: completed successfully (7ms)
```

- 增量
```shell
pgbackrest \
    --config /data/pgbackrest.conf \
    --stanza=pg01 \
    --log-level-console=info \
    --type=incr \
    backup

# 2024-04-02 09:03:07.010 P00   INFO: backup command begin 2.52dev: --config=/data/pgbackrest.conf --exec-id=293535-d811207f --log-level-console=info --log-path=/data/log --pg1-path=/var/lib/postgres/data/ --pg1-port=5432 --pg1-socket-path=/run/postgresql --pg1-user=postgres --process-max=3 --repo1-cipher-pass=<redacted> --repo1-cipher-type=aes-256-cbc --repo1-path=/data/pgbackup --repo1-retention-full=2 --stanza=pg01 --start-fast --type=incr
# 2024-04-02 09:03:07.722 P00   INFO: last backup label = 20240402-090120F, version = 2.52dev
# 2024-04-02 09:03:07.722 P00   INFO: execute non-exclusive backup start: backup begins after the requested immediate checkpoint completes
# 2024-04-02 09:03:08.423 P00   INFO: backup start archive = 00000001000000000000001F, lsn = 0/1F000028
# 2024-04-02 09:03:08.423 P00   INFO: check archive for prior segment 00000001000000000000001E
# 2024-04-02 09:03:11.791 P00   INFO: execute non-exclusive backup stop and wait for all WAL segments to archive
# 2024-04-02 09:03:11.992 P00   INFO: backup stop archive = 00000001000000000000001F, lsn = 0/1F000100
# 2024-04-02 09:03:11.994 P00   INFO: check archive for segment(s) 00000001000000000000001F:00000001000000000000001F
# 2024-04-02 09:03:12.008 P00   INFO: new backup label = 20240402-090120F_20240402-090307I
# 2024-04-02 09:03:12.043 P00   INFO: incr backup size = 151.4MB, file total = 979
# 2024-04-02 09:03:12.043 P00   INFO: backup command end: completed successfully (5036ms)
# 2024-04-02 09:03:12.043 P00   INFO: expire command begin 2.52dev: --config=/data/pgbackrest.conf --exec-id=293535-d811207f --log-level-console=info --log-path=/data/log --repo1-cipher-pass=<redacted> --repo1-cipher-type=aes-256-cbc --repo1-path=/data/pgbackup --repo1-retention-full=2 --stanza=pg01
# 2024-04-02 09:03:12.047 P00   INFO: repo1: 16-1 no archive to remove
# 2024-04-02 09:03:12.047 P00   INFO: expire command end: completed successfully (4ms)
```
