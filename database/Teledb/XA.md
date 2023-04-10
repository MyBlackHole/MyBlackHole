# XA

## 启用
```shell
# 8.0 默认启用
# version < 8.0
set innodb_support_xa=ON
```

## 是否支持 XA
```shell
show variables like 'innodb_support_xa';
+-------------------+-------+
| Variable_name     | Value |
+-------------------+-------+
| innodb_support_xa | ON    |
+-------------------+-------+
1 row in set
Time: 0.014s
```

## 例子
```shell
# 数据库\表 需要在 "分布式数据控制台" 创建
CREATE TABLE `article` (
`id` int(10) unsigned NOT NULL AUTO_INCREMENT,
`score` int(10) unsigned NOT NULL,
`name` varbinary(255) NOT NULL,
PRIMARY KEY (`id`)
);

MySQL test@192.168.90.207:UDALTEST> udal xa start '1000';
Reconnecting...
+--------------------+
| Xid                |
+--------------------+
| dbproxy-0746814c-1 |
+--------------------+
1 row in set
Time: 0.035s

MySQL test@192.168.90.207:UDALTEST> insert into article (score, `name`) values(10, "foo");
Query OK, 1 row affected
Time: 0.038s

MySQL test@192.168.90.207:UDALTEST> udal xa recover;
+------+--------------------+-----------+----------+----------------+
| Id   | Xid                | State     | Run_time | Frontend_state |
+------+--------------------+-----------+----------+----------------+
| 2175 | dbproxy-0746814c-1 | XA_ACTIVE | 482      | alive          |
+------+--------------------+-----------+----------+----------------+
1 row in set
Time: 0.064s

```
