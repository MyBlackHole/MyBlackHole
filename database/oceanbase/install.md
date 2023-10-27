# install

+ OBD: OceanBase Database Deployer社区版部署工具
+ oceanbase-ce: OceanBase数据库社区版
+ OceanBase libs: oceanbase运行时所依赖的部分三方动态库
+ Obproxy : oceanbase数据库专用的代理服务器
+ OBClient：obclient交互式和批量处理查询工具
+ LibOBClient：obclient的依赖包

## 单点
```shell
vim /etc/sysctl.conf
net.core.somaxconn = 2048
net.core.netdev_max_backlog = 10000
net.core.rmem_default = 16777216
net.core.wmem_default = 16777216
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216
net.ipv4.ip_local_port_range = 3500 65535
net.ipv4.ip_forward = 0
net.ipv4.conf.default.rp_filter = 1
net.ipv4.conf.default.accept_source_route = 0
net.ipv4.tcp_syncookies = 0
net.ipv4.tcp_rmem = 4096 87380 16777216
net.ipv4.tcp_wmem = 4096 65536 16777216
net.ipv4.tcp_max_syn_backlog = 16384
net.ipv4.tcp_fin_timeout = 15
net.ipv4.tcp_max_syn_backlog = 16384
net.ipv4.tcp_tw_reuse = 129 
net.ipv4.tcp_tw_recycle = 1
net.ipv4.tcp_slow_start_after_idle=0
vm.swappiness = 0
vm.min_free_kbytes = 2097152
vm.max_map_count=655360
fs.aio-max-nr=1048576

<!-- 使更改的配置生效 -->
sysctl -p

<!-- 修改 -->
vim /etc/security/limits.conf
* soft nofile 655360
* hard nofile 655360
* soft nproc 655360
* hard nproc 655360
* soft core unlimited
* hard core unlimited
* soft stack unlimited
* hard stack unlimited

<!-- 输出 -->
ulimit -a


<!-- 创建账号 -->
useradd admin
passwd admin

usermod admin -G wheel
id admin
uid=1000(admin) gid=1000(admin) groups=1000(admin),10(wheel)


<!-- 下载包 -->
wget https://mirrors.aliyun.com/oceanbase/community/stable/el/8/x86_64/oceanbase-ce-libs-3.1.0-3.el8.x86_64.rpm
wget https://mirrors.aliyun.com/oceanbase/community/stable/el/8/x86_64/obclient-2.0.0-2.el8.x86_64.rpm
wget https://mirrors.aliyun.com/oceanbase/community/stable/el/8/x86_64/obproxy-3.1.0-1.el8.x86_64.rpm
wget https://mirrors.aliyun.com/oceanbase/community/stable/el/8/x86_64/libobclient-2.0.0-2.el8.x86_64.rpm
wget https://mirrors.aliyun.com/oceanbase/community/stable/el/8/x86_64/oceanbase-ce-3.1.0-3.el8.x86_64.rpm

<!-- 安装 -->
rpm -ivh oc*.rpm
rpm -ivh libobclient-2.0.0-2.el8.x86_64.rpm
rpm -ivh ob*

/home/admin
tree oceanbase
oceanbase
|-- bin
|   |-- import_time_zone_info.py
|   `-- observer
|-- etc
|   `-- timezone_V1.log
`-- lib
   |-- libaio.so -> libaio.so.1.0.1
   |-- libaio.so.1 -> libaio.so.1.0.1
   |-- libaio.so.1.0.1
   |-- libmariadb.so -> libmariadb.so.3
   `-- libmariadb.so.3

3 directories, 8 files
 
tree obproxy-3.1.0
obproxy-3.1.0
`-- bin
   |-- obproxy
   `-- obproxyd.sh

1 directory, 2 files

<!-- 初始化目录 -->
mkdir -p ~/oceanbase/store/obdemo/{clog,etc2,etc3,ilog,slog,sstable}
tree
oceanbase
    └── store
      └── obdemo
        ├── clog
        ├── etc2
        ├── etc3
        ├── ilog
        ├── slog
        └── sstable


echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:~/oceanbase/lib' >> ~/.bash_profile
. ~/.bash_profile
cd ~/oceanbase && bin/observer -i lo -p 2881 -P 2882 -z zone1 -d ~/oceanbase/store/obdemo -r '127.0.0.1:2882:2881' -c 20210912  -n obdemo -o "memory_limit=4G,cache_wash_threshold=1G,__min_full_resource_pool_memory=268435456,system_memory=3G,memory_chunk_cache_size=128M,cpu_count=4,net_thread_count=4,datafile_size=10G,stack_size=1536K" -d ~/oceanbase/store/obdemo
bin/observer -i lo -p 2881 -P 2882 -z zone1 -d /home/admin/oceanbase/store/obdemo -r 127.0.0.1:2882:2881 -c 20210912 -n obdemo -o memory_limit=8G,cache_wash_threshold=1G,__min_full_resource_pool_memory=268435456,system_memory=3G,memory_chunk_cache_size=128M,cpu_count=4,net_thread_count=4,datafile_size=10G,stack_size=1536K -d /home/admin/oceanbase/store/obdemo
devname: enp0s3
mysql port: 2881
rpc port: 2882
zone: zone1
data_dir: /home/admin/oceanbase/store/obdemo
rs list: 127.0.0.1:2882:2881
cluster id: 20210912
appname: obdemo
optstr: memory_limit=8G,cache_wash_threshold=1G,__min_full_resource_pool_memory=268435456,system_memory=3G,memory_chunk_cache_size=128M,cpu_count=4,net_thread_count=4,datafile_size=10G,stack_size=1536K
data_dir: /home/admin/oceanbase/store/obdemo
 
ps -ef|grep obse
admin     1533     1 99 03:03 ?        00:00:13 bin/observer -i enp0s3 -p 2881 -P 2882 -z zone1 -d /home/admin/oceanbase/store/obdemo -r 127.0.0.1:2882:2881 -c 20210912 -n obdemo -o memory_limit=8G,cache_wash_threshold=1G,__min_full_resource_pool_memory=268435456,system_memory=3G,memory_chunk_cache_size=128M,cpu_count=4,net_thread_count=4,datafile_size=10G,stack_size=1536K -d /home/admin/oceanbase/store/obdemo
admin     1706  1452  0 03:03 pts/0    00:00:00 grep --color=auto obse

<!-- 连接(密码默认为空) -->
mysql -h127.0.0.1 -u root -P2881 -p -c -A

<!-- 执行集群自举 -->
set session ob_query_timeout=1000000000;
alter system bootstrap ZONE 'zone1' SERVER '127.0.0.1:2882';
 Query OK, 0 rows affected (1 min 57.52 sec)


<!-- 配置 obproxy 连接信息 -->
mysql -h127.0.0.1 -u root@sys -P2881 -p -c -A
grant select on oceanbase.* to proxyro identified by 'root123'; 创建代理用户
Query OK, 0 rows affected (0.20 sec)
alter user root identified by 'root123'; 更改root密码
Query OK, 0 rows affected (0.07 sec)

<!-- 启动 obproxy -->
cd ~/obproxy-3.1.0/ && bin/obproxy -r "127.0.0.1:2881" -p 2883 -o "enable_strict_kernel_release=false, enable_cluster_checkout=false,enable_metadb_used=false" -c obdemo
bin/obproxy -r 127.0.0.1 -p 2883 -o enable_strict_kernel_release=false,enable_cluster_checkout=false,enable_metadb_used=false -c obdemo
rs list: 127.0.0.1:2881
listen port: 2883
optstr: enable_strict_kernel_release=false,enable_cluster_checkout=false,enable_metadb_used=false
cluster_name: obdemo

obclient -h127.0.0.1 -u root@proxysys -P 2883 -p   ##登陆代理，初始密码为空
<!-- 修改obproxy sys用户密码 -->
alter proxyconfig set obproxy_sys_password='root123';
Query OK, 0 rows affected (0.003 sec)

<!-- 修改OBPROXY连接OceanBase集群用户proxyro密码，密码和前面创建用户proxyro的密码相同。 -->
alter proxyconfig set observer_sys_password='root123';
Query OK, 0 rows affected (0.002 sec)

```

<!-- 集群 -->
```shell
yum install -y yum-utils
yum-config-manager --add-repo https://mirrors.aliyun.com/oceanbase/OceanBase.repo

# 创建一个目录用于下载
mkdir soft
cat > rpm_list <<EOF
oceanbase-ce-3.1.0-3.el7.x86_64.rpm
oceanbase-ce-libs-3.1.0-3.el7.x86_64.rpm
obproxy-3.1.0-1.el7.x86_64.rpm
obclient-2.0.0-2.el7.x86_64.rpm
ob-deploy-1.1.0-1.el7.x86_64.rpm
libobclient-2.0.0-2.el7.x86_64.rpm
oceanbase-ce-sql-parser-3.1.0-3.el7.x86_64.rpm
EOF

wget -B https://mirrors.aliyun.com/oceanbase/community/stable/el/7/x86_64/ -i rpm_list -P soft

# or
yum install obclient libobclient obproxy oceanbase-ce oceanbase-ce-libs ob-deploy --downloadonly --downloaddir=soft


<!-- 关闭 selinux -->
<!-- 临时关闭 -->
setenforce 0
<!-- 检查 -->
getenforce

<!-- 开机不启动selinux，需重启OS生效。已临时关闭，本次不需要重启生效。 -->
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config

<!-- 查看配置已生效 -->
grep '^SELINUX=' /etc/selinux/config


<!-- 关闭防火墙 -->
systemctl stop firewalld

<!-- 开机禁用自启动防火墙并立刻关闭服务 -->
systemctl disable --now firewalld

<!-- 检查服务状态 -->
systemctl status firewalld 


<!-- 主机名解析添加主机信息 -->
cat >> /etc/hosts << EOF
192.168.10.182 oceanbase
EOF

<!-- 查看主机名信息 -->
cat /etc/hosts


<!-- 添加用户 -->
groupadd -g 1000 ober
useradd -u 1000 -g ober -m -d /ups/app/oceanbase -c "OceanBase Owner" ober


<!-- 更改密码 -->
echo "ober" | passwd --stdin ober
#运行oceanbase可运行任何命令，不需要密码

<!-- 配置 sudo 权限 -->
visudo      #添加oceanbase一行内容
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL
ober       ALL=(ALL)       NOPASSWD: ALL



cat > /etc/sysctl.d/98-obce.conf <<-'EOF'

# for oceanbase
## 修改内核异步 I/O 限制
fs.aio-max-nr=1048576

## 网络优化
net.core.somaxconn = 2048
net.core.netdev_max_backlog = 10000 
net.core.rmem_default = 16777216 
net.core.wmem_default = 16777216 
net.core.rmem_max = 16777216 
net.core.wmem_max = 16777216

net.ipv4.ip_local_port_range = 3500 65535 
net.ipv4.ip_forward = 0 
net.ipv4.conf.default.rp_filter = 1 
net.ipv4.conf.default.accept_source_route = 0 
net.ipv4.tcp_syncookies = 0 
net.ipv4.tcp_rmem = 4096 87380 16777216 
net.ipv4.tcp_wmem = 4096 65536 16777216 
net.ipv4.tcp_max_syn_backlog = 16384 
net.ipv4.tcp_fin_timeout = 15 
net.ipv4.tcp_max_syn_backlog = 16384 
net.ipv4.tcp_tw_reuse = 1 
net.ipv4.tcp_tw_recycle = 1 
net.ipv4.tcp_slow_start_after_idle=0

vm.swappiness = 0
vm.min_free_kbytes = 2097152
vm.max_map_count=655360

# 此处为 OceanBase 数据库的 data 目录
kernel.core_pattern = /data/core-%e-%p-%t
EOF

# 加载配置
sysctl --system



#添加内容
cat >> /etc/security/limits.d/98-ober.conf << EOF
* soft nofile 655350
* hard nofile 655350
* soft stack 20480
* hard stack 20480
* soft nproc 655360
* hard nproc 655360
* soft core unlimited
* hard core unlimited
EOF

#退出当前会话，重新登录，使配置生效。

#检查open files当前值，应为655350，否则后续启动集群会报错
ulimit -n


yum -y install chrony

vi /etc/chrony.conf
# server 后面跟时间同步服务器
# 使用pool.ntp.org 项目中的公共服务器。按 server 配置，理论上您想添加多少时间服务器都可以。
# 或者使用 阿里云的 ntp 服务器
# Please consider joining the pool (http://www.pool.ntp.org/join.html).
server ntp.cloud.aliyuncs.com minpoll 4 maxpoll 10 iburst
server ntp.aliyun.com minpoll 4 maxpoll 10 iburst
server ntp1.aliyun.com minpoll 4 maxpoll 10 iburst
server ntp1.cloud.aliyuncs.com minpoll 4 maxpoll 10 iburst
server ntp10.cloud.aliyuncs.com minpoll 4 maxpoll 10 iburst

# 如果是测试环境，没有时间同步服务器，那就选取一台配置为时间同步服务器。
# 如果选中的是本机，则取消下面 server 注释
#server 127.127.1.0

# 根据实际时间计算出服务器增减时间的比率，然后记录到一个文件中，在系统重启后为系统做出最佳时间补偿调整。
driftfile /var/lib/chrony/drift

# chronyd 根据需求减慢或加速时间调整，
# 在某些情况下系统时钟可能漂移过快，导致时间调整用时过长。
# 该指令强制 chronyd 调整时期，大于某个阀值时步进调整系统时钟。
# 只有在因 chronyd 启动时间超过指定的限制时（可使用负值来禁用限制）没有更多时钟更新时才生效。
makestep 1.0 3

# 将启用一个内核模式，在该模式中，系统时间每11分钟会拷贝到实时时钟（RTC）。
rtcsync

# Enable hardware timestamping on all interfaces that support it.
# 通过使用hwtimestamp指令启用硬件时间戳
#hwtimestamp eth0
#hwtimestamp eth1
#hwtimestamp *

# Increase the minimum number of selectable sources required to adjust
# the system clock.
#minsources 2

# 指定一台主机、子网，或者网络以允许或拒绝NTP连接到扮演时钟服务器的机器
#allow 192.168.0.0/16
#deny 192.168/16

# 即使没有同步到时间源，也要服务时间
local stratum 10

# 指定包含NTP验证密钥的文件。
#keyfile /etc/chrony.keys

# 指定日志文件的目录。
logdir /var/log/chrony


# Select which information is logged.
#log measurements statistics tracking


# 查看时间同步活动
chronyc activity

# 查看时间服务器
chronyc sources

# 查看同步状态
chronyc sources -v

# 校准时间服务器：
chronyc tracking

# 使用 clockdiff [-o] <host> 命令可以检查本机跟目标机器的时间同步误差
clockdiff 192.168.10.181



su - ober
cd /ups/app/oceanbase/cluster && bin/observer -i ens32 -l WARN -p 2881 -P 2882 -z zone1 -r '192.168.10.201:2882:2881;192.168.10.202:2882:2881;192.168.10.203:2882:2881' -c obce -n obce -o "__min_full_resource_pool_memory=268435456,memory_limit=8G,system_memory=4G,stack_size=512K,cpu_count=16,cache_wash_threshold=1G,workers_per_cpu_quota=10,schema_history_expire_time=1d,net_thread_count=4,major_freeze_duty_time=Disable,minor_freeze_times=10,enable_separate_sys_clog=0,enable_merge_by_turn=False,datafile_size=2G,enable_syslog_wf=False,enable_syslog_recycle=True,max_syslog_file_count=6" -d /ups/app/oceanbase/store/obce
su - ober
cd /ups/app/oceanbase/cluster && bin/observer -i ens32 -l WARN -p 2881 -P 2882 -z zone2 -r '192.168.10.201:2882:2881;192.168.10.202:2882:2881;192.168.10.203:2882:2881' -c obce -n obce -o "__min_full_resource_pool_memory=268435456,memory_limit=8G,system_memory=4G,stack_size=512K,cpu_count=16,cache_wash_threshold=1G,workers_per_cpu_quota=10,schema_history_expire_time=1d,net_thread_count=4,major_freeze_duty_time=Disable,minor_freeze_times=10,enable_separate_sys_clog=0,enable_merge_by_turn=False,datafile_size=2G,enable_syslog_wf=False,enable_syslog_recycle=True,max_syslog_file_count=6" -d /ups/app/oceanbase/store/obce
su - ober
cd /ups/app/oceanbase/cluster && bin/observer -i ens32 -l WARN -p 2881 -P 2882 -z zone3 -r '192.168.10.201:2882:2881;192.168.10.202:2882:2881;192.168.10.203:2882:2881' -c obce -n obce -o "__min_full_resource_pool_memory=268435456,memory_limit=8G,system_memory=4G,stack_size=512K,cpu_count=16,cache_wash_threshold=1G,workers_per_cpu_quota=10,schema_history_expire_time=1d,net_thread_count=4,major_freeze_duty_time=Disable,minor_freeze_times=10,enable_separate_sys_clog=0,enable_merge_by_turn=False,datafile_size=2G,enable_syslog_wf=False,enable_syslog_recycle=True,max_syslog_file_count=6" -d /ups/app/oceanbase/store/obce

netstat -ntlp|grep -E "2881|2882"


<!-- 初始化集群 -->
mysql -h 192.168.10.201 -u root -P 2881 -c -A -e "set session ob_query_timeout=1000000000; alter system bootstrap ZONE 'zone1' SERVER '192.168.10.201:2882', ZONE 'zone2' SERVER '192.168.10.202:2882', ZONE 'zone3' SERVER '192.168.10.203:2882' ;"

# obce 集群名称
cd /ups/app/oceanbase/obproxy && bin/obproxy -r "192.168.10.201:2881;192.168.10.202:2881;192.168.10.203:2881" -p 2883 -o "enable_strict_kernel_release=false,enable_cluster_checkout=false,skip_proxy_sys_private_check=True,enable_metadb_used=false" -l 2884 -n obce
```
