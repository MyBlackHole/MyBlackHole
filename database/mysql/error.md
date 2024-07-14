### 异常解决

- Invalid use of NULL value
```shell
修改表前，表源有数据不满足更新的条件
```

- mysqlbinlog: [ERROR] unknown variable 'default-character-set=utf8mb4'
```shell
# 加上 --no-defaults 
mysqlbinlog --no-defaults -v mysql-bin.000011 > binlog.sql

<!-- 或 -->

修改配置文件my.cnf，default-character-set=utf8mb4 修改为 character-set-server = utf8mb4，但是需要重启MySQL服务
```

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

- 忘记密码
```shell
# 关闭 mysqld

# 重新启动 mysqld 加上 --skip-grant-tables (不启用权限表验证)

use mysql;
UPDATE user SET authentication_string=PASSWORD("root") WHERE User="root"; # 修改密码
flush privileges; # 刷新权限

# 恢复原先 mysqld 启动参数
```

- root 没有 super 权限
```shell
# 重新启动 mysqld 加上 --skip-grant-tables (不启用权限表验证)

# 手动修改用户权限
UPDATE mysql.user SET Grant_priv='Y', Super_priv='Y' WHERE User='root';

# 刷新权限
FLUSH PRIVILEGES;

# 授权表
GRANT ALL ON *.* TO 'root'@'localhost';

# 查看用户权限
select * from mysql.user\G
```

- Got fatal error 1236 from master when reading data from binary log: 'binlog truncated in the middle of event; consider out of disk space on master; the first event 'mysql-bin.000004' at 154, the last event read from './mysql-bin.000004' at 126, the last byte read from './mysql-bin.000004' at 157.
```shell
stop slave
reset slave
start slave
```

- mysql 监听 tcp6
```shell
关闭ipv6方法：
方法1：编辑/etc/sysctl.conf文件，添加如下两行到文件
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
如果想只关闭某个网卡的ipv6地址呢，比如说关闭eth0的ipv6地址：还是修改/etc/sysctl.conf文件，添加如下配置：
net.ipv6.conf.eth0.disable_ipv6 = 1
保存退出，使用sysctl -p命令使配置生效

方法2：
echo 1 > /proc/sys/net/ipv6/conf/all/disable_ipv6
echo 1 > /proc/sys/net/ipv6/conf/default/disable_ipv6
或，
sysctl -w net.ipv6.conf.all.disable_ipv6=1
sysctl -w net.ipv6.conf.default.disable_ipv6=1

配置mysql的配置文件/etc/my.conf，在mysqld下面添加bind-address=0.0.0.0 然后重新启动mysql服务就能正常监听ibv4地址了
```


