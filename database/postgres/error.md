# 异常处理

- 数据库 "black" 不存在
```shell
psql -U black 

# # 原因
# 连接时默认会指定连接到black数据库
# initdb 初始化数据库时,未指定用户不会自动创建对应数据库

# 解决

psql -U black -d template1
pgcli -h 127.0.0.1 -u black -d template1
```

- psql: FATAL:  Peer authentication failed for user "my_user"
```shell
sudo nvim /etc/postgresql/14/main/pg_hba.conf

local   all             postgres                                peer
# 修改为
local   all             postgres                                md5

sudo systemctl restart postgresql

```


- How to fix 'postgres.h' file not found problem
```shell
sudo apt install postgresql-server-dev-15
```
