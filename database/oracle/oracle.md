# oracle

## install
```shell
# 拉取镜像
docker pull iatebes/oracle_11g

# 启动为容器
docker run -d -p 1521:1521 --name oracle11g iatebes/oracle_11g

# 进入容器
docker exec -it oracle11g /bin/bash

# 连接可执行文件
ln -s /opt/oracle/app/product/11.2.0/dbhome_1/bin/sqlplus /usr/bin

# 切换用户
su - oracle

# 启动客户端
sqlplus /nolog
conn /as sysdba; # 以系统管理员身份登录
alter user system identified by system; # system用户密码修改为system
alter user sys identified by sysdba; # sys用户密码修改为sysdba
ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED; # 修改密码规则策略为密码永不过期
alter system set processes=1000 scope=spfile; # 修改数据库最大连接数
 
exit; -- 退出


sqlplus /nolog
conn /as sysdba; # 以系统管理员身份登录
shutdown immediate; # 关闭数据库, 需要管理员身份登录
startup; # 重启数据库

# 重启后查看service_names，发现默认service_names为orcl
show parameter name; # 查看参数：service_names为orcl

# 修改service_names为自己想要的名称
alter system set service_names=mydb scope=both; # 修改service_names
```
