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
