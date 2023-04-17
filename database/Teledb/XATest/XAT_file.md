# XAT
XA 事务

xa 数据目录： dbproxy 部署节点的 /teledb/dbproxy/xa/data 路径

## 开启 file
实例列表 -- 管理 -- 分组管理 -- 属性设置 -- 查询属性**xaStorage** -- 修改属性值 **file**
重启 dbproxy (不会热生效)

## 表
```sql
/*建表语句*/
CREATE TABLE IF NOT EXISTS `user` (
`id` int(10) unsigned NOT NULL AUTO_INCREMENT,
`score` int(10) unsigned NOT NULL,
`name` varbinary(255) NOT NULL,
PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
/*sharding语句*/
sharding @@table name='user' set type='sharding' and sharding_algo='PartitionByMod' and sharding_id='id' and dn='udaltest_1,udaltest_2,udaltest_3';
# 三个分片，通过 id mod
```

## 验证 xa 使用 rocksdb 存放
### 验证单次 xa 单条输入
单条数据插入不进行 xa xid 记录
- 初始状态
```shell
# 数据目录状态
[teledb@teledb04 data]$ ls
000053.sst  000085.log  CURRENT   LOCK  LOG.old.1681453247935868  OPTIONS-000008
000055.sst  000087.sst  IDENTITY  LOG   MANIFEST-000056           OPTIONS-000059

# 日志状态
[teledb@teledb04 data]$ tail -f 000085.log

udaltest_2(dbproxy-b2d4b50e-69938-udaltest_2-3064476

udaltest_1(dbproxy-b2d4b50e-69938-udaltest_1-307357dbproxy-b2d4b50e-699386

udaltest_3(dbproxy-b2d4b50e-69938-udaltest_3-3063036

udaltest_2(dbproxy-b2d4b50e-69938-udaltest_2-3064476

udaltest_1(dbproxy-b2d4b50e-69938-udaltest_1-307357]odbproxy-b2d4b50e-69938
```
- 执行 sql 
```sql
MySQL test@192.168.90.207:UDALTEST> UDAL XA START;
+------------------------+
| Xid                    |
+------------------------+
| dbproxy-b2d4b50e-69939 |
+------------------------+
1 row in set
Time: 0.074s
MySQL test@192.168.90.207:UDALTEST> insert into user (score, name) values(RAND() * 100, 'test1');
Query OK, 1 row affected
Time: 0.051s
MySQL test@192.168.90.207:UDALTEST> commit;
Query OK, 0 rows affected
Time: 0.010s
MySQL test@192.168.90.207:UDALTEST>
```

- 执行 sql 后状态
```shell
[teledb@teledb04 data]$ tail -f 000085.log

udaltest_2(dbproxy-b2d4b50e-69938-udaltest_2-3064476

udaltest_1(dbproxy-b2d4b50e-69938-udaltest_1-307357dbproxy-b2d4b50e-699386

udaltest_3(dbproxy-b2d4b50e-69938-udaltest_3-3063036

udaltest_2(dbproxy-b2d4b50e-69938-udaltest_2-3064476

udaltest_1(dbproxy-b2d4b50e-69938-udaltest_1-307357]odbproxy-b2d4b50e-69938
```

### 验证单次 xa 插入两条
插入两条数据插入进行 xa xid 记录
- 初始状态
```shell
# 数据目录状态
[teledb@teledb04 data]$ ls
000053.sst  000085.log  CURRENT   LOCK  LOG.old.1681453247935868  OPTIONS-000008
000055.sst  000087.sst  IDENTITY  LOG   MANIFEST-000056           OPTIONS-000059

# 日志状态
[teledb@teledb04 data]$ tail -f 000085.log

udaltest_2(dbproxy-b2d4b50e-69938-udaltest_2-3064476

udaltest_1(dbproxy-b2d4b50e-69938-udaltest_1-307357dbproxy-b2d4b50e-699386

udaltest_3(dbproxy-b2d4b50e-69938-udaltest_3-3063036

udaltest_2(dbproxy-b2d4b50e-69938-udaltest_2-3064476

udaltest_1(dbproxy-b2d4b50e-69938-udaltest_1-307357]odbproxy-b2d4b50e-69938
```
- 执行 sql
```sql
MySQL test@192.168.90.207:UDALTEST> UDAL XA START;
+------------------------+
| Xid                    |
+------------------------+
| dbproxy-b2d4b50e-69940 |
+------------------------+
1 row in set
Time: 0.039s
MySQL test@192.168.90.207:UDALTEST> insert into user (score, name) values(RAND() * 100, 'test1');
Query OK, 1 row affected
Time: 0.014s
MySQL test@192.168.90.207:UDALTEST> insert into user (score, name) values(RAND() * 100, 'test1');
Query OK, 1 row affected
Time: 0.093s
MySQL test@192.168.90.207:UDALTEST> commit;
Query OK, 0 rows affected
Time: 0.011s
```

- 执行后日志状态
```shell
[teledb@teledb04 data]$ tail -f 000085.log

udaltest_2(dbproxy-b2d4b50e-69938-udaltest_2-3064476

udaltest_1(dbproxy-b2d4b50e-69938-udaltest_1-307357dbproxy-b2d4b50e-699386

udaltest_3(dbproxy-b2d4b50e-69938-udaltest_3-3063036

udaltest_2(dbproxy-b2d4b50e-69938-udaltest_2-3064476

udaltest_1(dbproxy-b2d4b50e-69938-udaltest_1-307357]odbproxy-b2d4b50e-69938dbproxy-b2d4b50e-69940r6

udaltest_3(dbproxy-b2d4b50e-69940-udaltest_3-3063036

udaltest_2(dbproxy-b2d4b50e-69940-udaltest_2-306447>dbproxy-b2d4b50e-69940r6

udaltest_3(dbproxy-b2d4b50e-69940-udaltest_3-3063036

udaltest_2(dbproxy-b2d4b50e-69940-udaltest_2-306447U5dbproxy-b2d4b50e-69940
```

### 测试是否是出现多分片时记录
确实是同分片时不记录 xa xid
单分片时依赖与 DN xa
- 日志初始状态
```shell
[teledb@teledb04 data]$ tail -f 000085.log

udaltest_3(dbproxy-b2d4b50e-69940-udaltest_3-3063036

udaltest_2(dbproxy-b2d4b50e-69940-udaltest_2-306447>dbproxy-b2d4b50e-69940r6

udaltest_3(dbproxy-b2d4b50e-69940-udaltest_3-3063036

udaltest_2(dbproxy-b2d4b50e-69940-udaltest_2-306447U5dbproxy-b2d4b50e-69940
```

- 执行插入同分片
```sql
MySQL test@192.168.90.207:UDALTEST> insert into user (id, score, name) values(908760, RAND() * 100, 'test1');
Query OK, 1 row affected
Time: 0.433s
MySQL test@192.168.90.207:UDALTEST> insert into user (id, score, name) values(908763, RAND() * 100, 'test1');
Query OK, 1 row affected
Time: 0.005s
MySQL test@192.168.90.207:UDALTEST> COMMIT;
Query OK, 0 rows affected
Time: 0.054s
```

- 执行同分片插入后日志状态
```shell
[teledb@teledb04 data]$ tail -f 000085.log

udaltest_3(dbproxy-b2d4b50e-69940-udaltest_3-3063036

udaltest_2(dbproxy-b2d4b50e-69940-udaltest_2-306447>dbproxy-b2d4b50e-69940r6

udaltest_3(dbproxy-b2d4b50e-69940-udaltest_3-3063036

udaltest_2(dbproxy-b2d4b50e-69940-udaltest_2-306447U5dbproxy-b2d4b50e-69940
```

- 执行插入不同分片
```sql
MySQL test@192.168.90.207:UDALTEST> UDAL XA START;
+------------------------+
| Xid                    |
+------------------------+
| dbproxy-b2d4b50e-69942 |
+------------------------+
1 row in set
Time: 0.013s
MySQL test@192.168.90.207:UDALTEST> insert into user (id, score, name) values(908764, RAND() * 100, 'test1');
Query OK, 1 row affected
Time: 0.016s
MySQL test@192.168.90.207:UDALTEST> insert into user (id, score, name) values(908765, RAND() * 100, 'test1');
Query OK, 1 row affected
Time: 0.075s
MySQL test@192.168.90.207:UDALTEST> COMMIT;
Query OK, 0 rows affected
Time: 0.007s
```

- 执行不同分片插入后日志状态
```shell
[teledb@teledb04 data]$ tail -f 000085.log

udaltest_2(dbproxy-b2d4b50e-69940-udaltest_2-306447>dbproxy-b2d4b50e-69940r6

udaltest_3(dbproxy-b2d4b50e-69940-udaltest_3-3063036

udaltest_2(dbproxy-b2d4b50e-69940-udaltest_2-306447U5dbproxy-b2d4b50e-699400dbproxy-b2d4b50e-69942r6

udaltest_3(dbproxy-b2d4b50e-69942-udaltest_3-3072286

udaltest_2(dbproxy-b2d4b50e-69942-udaltest_2-307370dbproxy-b2d4b50e-69942r6

udaltest_3(dbproxy-b2d4b50e-69942-udaltest_3-3072286

udaltest_2(dbproxy-b2d4b50e-69942-udaltest_2-307370lYdbproxy-b2d4b50e-69942
```

### 高并发验证
```shell
# 监控 
tail -f 0000xx.log

# 执行 XA 高并发程序
# 监控到
udaltest_3^R(dbproxy-b2d4b50e-69206-udaltest_3-301449^A^Vdbproxy-b2d4b50e-69177r^P^B^Z6
# 类似输出
```

