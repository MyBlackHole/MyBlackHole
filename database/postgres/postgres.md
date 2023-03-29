# postgres
开源数据库

## 安装
```shell
# ubuntu 
sudo apt install postgresql postgresql-contrib
```

## 配置
```shell
# ubuntu
sudo su postgres
psql

## 修改默认账号密码
alter user postgres with password '1234';

## 创建用户
create user black with password '1234';

## 给新建用户管理员权限
alter user black with superuser;

## 查询账户信息
\du;

## black 可以 pgcli 登陆
pgcli -U black -d template1 -W
```
