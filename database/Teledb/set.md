# set
master -- slave

```shell
# 安装脚本路径
/teledb/telemonitor-stg/telemonitor/python/manualdeploy/tianyiinstall_mysql.sh
```

## dn
具有全部权限的 user host
+-----------+-----------+
| host      | user      |
+-----------+-----------+
| localhost | root      |
| localhost | RDS_agent |
+-----------+-----------+

- root 权限
| GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, PROCESS, FILE, REFERENCES, INDEX, ALTER, SHOW DATABASES, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, CREATE USER, EVENT, TRIGGER ON *.* TO 'root'@'%' WITH GRANT OPTION |

- 单点 mysql 连接
```shell
mysql --socket=/tmp/mysql_5555.sock -u RDS_agent -pReport25Gky8@#*

mycli -h 192.168.90.204 -P 5555 -u root -pMax25Gky8@#*

# 具备 super 权限
mycli -h 192.168.90.204 -P 5555 -u telemonitor -pPython25Gky8@#*
```

- 给 test 创建配置 xtrabackup 备份最小权限
```shell
# # 登陆具备创建账号权限用户
# mycli -h 192.168.90.204 -P 5555 -u root -pMax25Gky8@#*
# # 创建用户
# create USER test@'%' identified by '1358';
# # 设置权限 (这里无法直接设置 super 权限)
# GRANT RELOAD, LOCK TABLES, PROCESS, REPLICATION CLIENT, REPLICATION SLAVE ON *.* TO 'test'@'%';
# flush privileges;

# 204 master
mysql --socket=/tmp/mysql_5555.sock -u RDS_agent -pReport25Gky8@#*
create USER test@'%' identified by '1358';
GRANT RELOAD, LOCK TABLES, PROCESS, REPLICATION CLIENT, REPLICATION SLAVE, SUPER ON *.* TO 'test'@'%';
flush privileges;


# 205 master
mysql --socket=/tmp/mysql_6666.sock -u RDS_agent -pReport25Gky8@#*
create USER test@'%' identified by '1358';
GRANT RELOAD, LOCK TABLES, PROCESS, REPLICATION CLIENT, REPLICATION SLAVE, SUPER ON *.* TO 'test'@'%';
flush privileges;

# 206 master
mysql --socket=/tmp/mysql_7777.sock -u RDS_agent -pReport25Gky8@#*
create USER test@'%' identified by '1358';
GRANT RELOAD, LOCK TABLES, PROCESS, REPLICATION CLIENT, REPLICATION SLAVE, SUPER ON *.* TO 'test'@'%';
flush privileges;
```
