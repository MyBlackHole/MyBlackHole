### 异常解决

- Can't read dir of '/etc/mysql/conf.d/
```shell
# 启动一个没有挂载的mysql容器
docker run --rm -it --name strange_almeida mysql:5.7 /bin/bash
bash-4.2# cd /etc/mysql/
bash-4.2# ls
conf.d	mysql.conf.d
# 将文件夹下的目录 拷贝出来，最好是挂载的目录，后面根据自己挂载路径进行调试
docker cp strange_almeida:/etc/mysql/ /data/docker/mysql/conf/
# 接着再次运行，可以正常启动了
```

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

- (1399, 'XAER_RMFAIL: The command cannot be executed when global transaction is in the  NON-EXISTING state')
```shell
xa 开启与解散事务不在一个 session
# 就是
xa start xid
xa end xid
```

- tx_mysql_thread_id 为 0
```shell
是 xa 事务
需要 xa 回滚或提交
```
