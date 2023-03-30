# pg_waldump

为了调试，解码并显示PostgreSQL预写日志.

使用方法:
  pg_waldump [选项]... [STARTSEG [ENDSEG]]

选项:
  -b, --bkp-details      输出有关备份块的详细信息
  -B, --block=N          with --relation, only show records that modify block N
  -e, --end=RECPTR       在指定的WAL位置停止读取
  -f, --follow           在到达可用WAL的末尾之后，继续重试
  -F, --fork=FORK        only show records that modify blocks in fork FORK;
                         valid names are main, fsm, vm, init
  -n, --limit=N          要显示的记录数
  -p, --path=PATH        directory in which to find WAL segment files or a
                         directory with a ./pg_wal that contains such files
                         (default: current directory, ./pg_wal, $PGDATA/pg_wal)
  -q, --quiet            不打印任何输出，错误除外
  -r, --rmgr=RMGR        只显示由RMGR资源管理器生成的记录
                         使用--rmgr=list列出有效的资源管理器名称
  -R, --relation=T/D/R   only show records that modify blocks in relation T/D/R
  -s, --start=RECPTR     在WAL中位于RECPTR处开始阅读
  -t, --timeline=TLI     timeline from which to read WAL records
                         (default: 1 or the value used in STARTSEG)
  -V, --version          输出版本信息, 然后退出
  -w, --fullpage         only show records with a full page write
      --save-fullpage=PATH
                         save full page images
  -x, --xid=XID          只显示用给定事务ID标记的记录
  -z, --stats[=record]   显示统计信息而不是记录
                         (或者，显示每个记录的统计信息)
  -?, --help             显示此帮助, 然后退出


## 例子
- 解析日志
```shell
./pg_waldump /opt/postgres15/data/pg_wal/000000010000000000000001
```

- wal 统计
```shell
pg_waldump -p $PGDATA/pg_wal -z  -s    0/8E48C2B0 -e 0/9E392748
```
