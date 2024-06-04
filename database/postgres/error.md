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

- 密码忘记
```shell
nvim /etc/postgresql/15/main/pg_hba.conf
修改对应连接方式的 method 为 trust 

<!-- 原:scram-sha-256 -->
<!-- 这里修改 IPv4\IPv6 -->
# IPv4 local connections:
host    all             all             127.0.0.1/32            trust
# IPv6 local connections:
host    all             all             ::1/128                 trust


<!-- 连接 -->
psql -h 127.0.0.1 -p 5433 -U postgres

psql -h 127.0.0.1 -p 5433 -U postgres
<!-- 修改密码 -->
alter user postgres with password '12345678';
```

- mmap(150994944) with MAP_HUGETLB failed, huge pages disabled: 无法分配内存
```shell
❯ cat /proc/meminfo|grep Huge
AnonHugePages:      8192 kB
ShmemHugePages:        0 kB
FileHugePages:         0 kB
HugePages_Total:       0
HugePages_Free:        0
HugePages_Rsvd:        0
HugePages_Surp:        0
Hugepagesize:       2048 kB
Hugetlb:               0 kB

sudo sysctl vm.nr_hugepages=128

❯ cat /proc/meminfo|grep Huge
AnonHugePages:      8192 kB
ShmemHugePages:        0 kB
FileHugePages:         0 kB
HugePages_Total:     128
HugePages_Free:      128
HugePages_Rsvd:        0
HugePages_Surp:        0
Hugepagesize:       2048 kB
Hugetlb:          262144 kB
```

- 忽略系统索引 (pg_class_oid_index 异常)
```shell
echo "ignore_system_indexes='true'" >> $PGDATA/postgresql.auto.conf
```
