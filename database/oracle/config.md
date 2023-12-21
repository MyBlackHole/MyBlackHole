# config

- 获取用户拥有的表
```shell
SELECT TABLE_NAME FROM USER_TABLES;
```

- 用户可存取的表
```shell
SELECT TABLE_NAME FROM ALL_TABLES;
```

- 数据库中所有表
```shell
SELECT TABLE_NAME FROM DBA_TABLES;
select * from tab;
```

- 查看当前所有的数据库
```shell
select * from v$database;

select name from v$database;
```

- 查看哪些用户拥有 sysdba、sysoper 权限
```shell
select * from V$PWFILE_USERS;
```

- 查看数据库结构
```shell
desc v$database;
```

- 查看表结构
```shell
desc 表名;

desc user$;
```

- 更改数据库用户密码
```shell
alter user 用户名 identified by 密码；
```

- 增加数据库用户
```shell
create user 用户名 identified by 密码 default tablespace users Temporary TABLESPACE Temp；
```

- 用户授权
```shell
grant connect，resource，dba to 用户名；
grant sysdba to 用户名；
```

- dba 文件信息
```shell
select tablespace_name,
       file_id,
       file_name,
       round(bytes / (1024 * 1024*1024), 0)  || 'G'  total_space
  from dba_data_files
 order by tablespace_name;
```

- 临时表空间
```shell
select tablespace_name,file_name,bytes/1024/1024 "file_size(M)",autoextensible from dba_temp_files;
```

- 是否处于归档模式
```shell
SELECT log_mode FROM v$database; 
```

- sga
```shell
select * from v$sga;
```
