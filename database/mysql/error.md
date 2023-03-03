### 异常解决

- mysql5.7导入DATETIME字段数据时遇到错误incorrect datetime value '0000-0-0 00:00:00' for column 

```sql
CREATE TABLE `t_tag_post` ( 
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT, 
  `pay_time` datetime NOT NULL DEFAULT '0000-00-00 00:00:00' COMMENT '发布时间', 
  PRIMARY KEY (`id`), 
  KEY `tagid` (`tag_id`), 
  KEY `postid` (`post_id`) 
) ENGINE=InnoDB AUTO_INCREMENT=13 DEFAULT CHARSET=utf8; 

show variables like 'sql_mode'; 
 
+---------------+------------------------------------------------------------------------------------------------------------------------------------+ 
| variable_name | value                                                                                                                                                                                        | 
+---------------+------------------------------------------------------------------------------------------------------------------------------------+ 
| sql_mode        | ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION | 
+---------------+-------------------------------------------------------------------------------------------------------------------------------------+ 
1 row in set (0.00 sec) 

# 解决办法：去掉NO_ZERO_IN_DATE,NO_ZERO_DATE 

set global sql_mode='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION'; 
```

- mysql 客户端SSL错误2026 (HY000) 
```shell
mysql -h10.233.117.225 -P3306 -uroot -p --ssl-mode=DISABLED
```
