# install

- 源码编译
```shell
sudo apt-get install libldap2-dev ldap-utils
sudo apt-get install libsasl2-dev

mkdir bulid
cd bulid

# 需要 openssl 1.x.x
cmake -DCMAKE_BUILD_TYPE=Debug \
      -DCMAKE_EXPORT_COMPILE_COMMANDS=1 \
      -DCMAKE_C_FLAGS_DEBUG="-O0 -g" \
      -DCMAKE_CXX_FLAGS_DEBUG="-O0 -g" \
      -DCMAKE_PREFIX_PATH=/media/black/Data/lib/openssl/openssl_1_1_1/ \
      -DDOWNLOAD_BOOST=1 -DWITH_BOOST=/media/black/Data/lib/boost ..

# cmake -DCMAKE_BUILD_TYPE=Debug \
#       -DCMAKE_EXPORT_COMPILE_COMMANDS=1 \
#       -DCMAKE_C_FLAGS_DEBUG="-O0 -g" \
#       -DCMAKE_CXX_FLAGS_DEBUG="-O0 -g" \
#       -DWITH_SSL=/media/black/Data/lib/openssl \
#       -DDOWNLOAD_BOOST=1 -DWITH_BOOST=/media/black/Data/lib/boost ..

# cmake -DCMAKE_BUILD_TYPE=Debug \
#       -DCMAKE_EXPORT_COMPILE_COMMANDS=1 \
#       -DCMAKE_C_FLAGS_DEBUG="-O0 -g" \
#       -DCMAKE_CXX_FLAGS_DEBUG="-O0 -g" \
#       -DDOWNLOAD_BOOST=1 -DWITH_BOOST=/media/black/Data/lib/boost ..
```

- podman 版
```shell
# 拉去镜像
podman pull mysql

# 启动
```shell
# 异常
#sudo chowm 999:999 data conf logs
    <!-- --privileged=true \ -->

podman run -p 3306:3306 --name mysql \
    -v /etc/localtime:/etc/localtime:ro \
    -v /opt/podman/mysql5/conf:/etc/mysql \
    -v /opt/podman/mysql5/logs:/var/log/mysql \
    -v /opt/podman/mysql5/data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=88888888 \
    -d mysql

# 修改data、logs、conf 用户与用户组为 999:999
#sudo chowm 999:999 data conf logs
podman run -p 3306:3306 --name mysql \
    -v /etc/localtime:/etc/localtime:ro \
    -v $(pwd)/test_mysql/conf:/etc/mysql \
    -v $(pwd)/test_mysql/logs:/var/log/mysql \
    -v $(pwd)/test_mysql/data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=123456 \
    -d mysql

# 或
podman run -it --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=p@3Sw0rd -d mysql

# 或

mkdir -p /opt/podman/mysql/conf/conf.d
podman run -p 3306:3306 --name mysql \
    -v /etc/localtime:/etc/localtime:ro \
    -v /opt/podman/mysql/conf:/etc/mysql \
    -v /opt/podman/mysql/logs:/var/log/mysql \
    -v /opt/podman/mysql/data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=88888888 \
    -d mysql:8
```

- 物理机版
```shell
# centos
## 5.7
wget http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm

## 8.0
wget https://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm

yum instal ./mysql80-community-release-el7-1.noarch.rpm 

<!-- gpg --export -a 3a79bd29 > 3a79bd29.asc -->
<!-- rpm --import 3a79bd29.asc -->
rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022


yum install mysql-community-server.x86_64 
service mysqld start      //启动mysql 
service mysqld status   //查看mysql状态 
systemctl enable mysqld //配置开机启动 
grep 'temporary password' /var/log/mysqld.log  //找到默认密码
mysql -uroot -p
alter user 'root'@'localhost' identified by 'p@3Sw0rd'; (强制必须修改)
create USER root@'%' identified by 'p@3Sw0rd';

# ubuntu
sudo apt-get update 
sudo apt-get install mysql-server 
sudo apt install mysql-workbench（可视化界面） 
service mysql start 
service mysql stop 

# win
https://cdn.mysql.com/Downloads/MySQL-8.0/mysql-8.0.26-winx64.msi 
```


- archlinux
```shell
paru -S percona-server-clients percona-server
systemctl start mysqld
sudo nvim /var/log/mysqld.log  # 查看默认密码

alter user 'root'@'localhost' identified with mysql_native_password by 'xxxx';
flush privileges;

sudo usermod -G black mysql
```
