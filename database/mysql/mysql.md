### 安装、权限、用户
- docker 版
```shell
# 拉去镜像
docker pull mysql
# 启动
duso docker run -p 3306:3306 --name mysql \  
    -v /data/docker/mysql/conf:/etc/mysql \  
    -v /data/docker/mysql/logs:/var/log/mysql \  
    -v /data/docker/mysql/data:/var/lib/mysql \  
    -e MYSQL_ROOT_PASSWORD=123456 \ -d mysql:5.7 

# 或
docker run -it --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag 

# 连接 mysql
mysql -h 127.0.0.1 -P 3306 -uroot –p 
```

- 物理机版
```shell
# centos
wget https://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm 
sudo yum instal   mysql80-community-release-el7-1.noarch.rpm 
yum install mysql-community-server.x86_64 
sudo service mysqld start      //启动mysql 
sudo service mysqld status   //查看mysql状态 
sudo systemctl enable mysqld //配置开机启动 
grep 'temporary password'/var/log/mysqld.log  //找到默认密码 
mysql -uroot -p  
setpassword for'root'@'localhost'=password('NEWPASSWORD'); 
或者ALTERUSER 'root'@'localhost'IDENTIFIEDBY 'NEWPASSWORD';// 修改密码,注意密码要复杂一些，否则会不能通过。 
alter user user()identified by "XXXXXX"; 

# ubuntu
sudo apt-get update 
sudo apt-get install mysql-server 
sudo apt install mysql-workbench（可视化界面） 
service mysql start 
service mysql stop 

# win
https://cdn.mysql.com/Downloads/MySQL-8.0/mysql-8.0.26-winx64.msi 
```

- 用户配置
创建用户，授予远程登录权限、操作所有数据权限
create USER BlackHole@'%' identified by '1358244533';   
grant all on *.* to BlackHole@'%' with grant option; 
flush privileges; 

- 授权
    格式
    grant privilegesCode on dbName.tableName to username@host by "password"; 
    (8.0版本，8.0前加上privileges 在on前 ) 

    privilegesCode 
        all ：所有权限。 
        select：读取权限。 
        delete：删除权限。 
        update：更新权限。 
        create：创建权限。 
        drop：删除数据库、数据表权限。 
    dbName.tableName 
        *.*：授予该数据库服务器所有数据库的权限。 
        dbName.*：授予dbName数据库所有表的权限。 
        dbName.dbTable：授予数据库dbName中dbTable表的权限。 
    Host 
        localhost：只允许该用户在本地登录，不能远程登录。 
        %：允许在除本机之外的任何一台机器远程登录。 
        192.168.52.32：具体的IP表示只允许该用户从特定IP登录。 

```sql
# 例如
# 8.0前
grant all privileges on minidome.* to Black@'%' identified by '1358244533';  # 授权Blcak具有远程登陆与minidome数据库的所有权限 
# 8.0后
create USER BlackHole@'%' identified by '1358244533';  # 创建具有远程登陆的BlackHole用户 
grant all on minidome.* to BlackHole@'%' with grant option;  # 授予BlackHole具有minidome的所有权限 
flush privileges; # 表示刷新权限变更 
show grants for 'heidong'; # 查看权限授予
```

- 创建账号
```sql
create USER BlackHole@'%' identified by '1358244533';  # 创建具有远程登陆的BlackHole用户
```

- 删除
```sql
drop user BlackHole@'localhost';  # 删除用户及权限 
drop database minidome; # 删除数据库
Delete FROM user Where User='Black' and Host='localhost'  # 删除用户
```

- 更新
```sql
update mysql.user set password=password('新密码') where User="BlackHole" and Host="localhost"; # 更新密码
```

