# mysqld_exporter

```shell
<!-- create user 'exporter'@'%' identified by 'MONty_00'; -->
<!-- GRANT PROCESS, REPLICATION CLIENT, SELECT ON *.* TO 'exporter'@'%'  WITH MAX_USER_CONNECTIONS 3; -->
<!-- flush privileges; -->

lvim .my.cnf
[client]
host=127.0.0.1
port=3306
user=root
password=****


./mysqld_exporter --config.my-cnf=".my.cnf"


export DATA_SOURCE_NAME='user:password@(hostname:3306)/'
./mysqld_exporter <flags>
```

