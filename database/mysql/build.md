# build

```bash
paru -S rpcsvc-proto

mkdir build
cd build
cmake -DDOWNLOAD_BOOST=1 \
      -DWITH_BOOST=dwith_boost \
      -DCMAKE_EXPORT_COMPILE_COMMANDS=1 \
      -DCMAKE_BUILD_TYPE=Debug \
      -DCMAKE_C_FLAGS_DEBUG="-O0" \
      -DCMAKE_CXX_FLAGS_DEBUG="-O0" ..
# DCMAKE_EXPORT_COMPILE_COMMANDS 启用编译数据库文件生成
# DCMAKE_BUILD_TYPE 启用 -g
# DCMAKE_C_FLAGS_DEBUG 关闭编译优化

make -j 4

lvim my.cnf
[client]
port=13306
socket=/run/media/black/Data/Documents/github/cpp/mysql-server/mysql_data/mysql.sock
default-character-set = utf8mb4

[mysqld]
default-time_zone = '+8:00'

port= 13306
user= black
character-set-server = utf8mb4
basedir = /run/media/black/Data/Documents/github/cpp/mysql-server/mysql_data/use/
datadir=/run/media/black/Data/Documents/github/cpp/mysql-server/mysql_data/data/
pid-file=/run/media/black/Data/Documents/github/cpp/mysql-server/mysql_data/mysql.pid
socket=/run/media/black/Data/Documents/github/cpp/mysql-server/mysql_data/mysql.sock


# 初始化
./mysql_data/use/bin/mysqld --defaults-file=/run/media/black/Data/Documents/github/cpp/mysql-server/mysql_data/my.cnf --initialize-insecure --user black

# 启动
./mysql_data/use/bin/mysqld --defaults-file=/run/media/black/Data/Documents/github/cpp/mysql-server/mysql_data/my.cnf


mysql -u root -P13306
```
