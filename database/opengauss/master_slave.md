# master slave

## docker
```shell
docker pull enmotech/opengauss:5.0.0
wget https://raw.githubusercontent.com/enmotech/enmotech-docker-opengauss/master/create_master_slave.sh
chmod u+x create_master_slave.sh 

Please input OG_SUBNET (容器所在网段) [172.11.0.0/24]:
OG_SUBNET set 172.11.0.0/24
Please input GS_PASSWORD (定义数据库密码)[Enmo@123]:
GS_PASSWORD set Enmo@123
Please input MASTER_IP (主库IP)[172.11.0.101]:
MASTER_IP set 172.11.0.101
Please input SLAVE_1_IP (备库IP)[172.11.0.102]:
SLAVE_1_IP set 172.11.0.102
Please input MASTER_HOST_PORT (主库数据库服务端口)[5432]:
MASTER_HOST_PORT set 5432
Please input MASTER_LOCAL_PORT (主库通信端口)[5434]:
MASTER_LOCAL_PORT set 5434
Please input SLAVE_1_HOST_PORT (备库数据库服务端口)[6432]:
SLAVE_1_HOST_PORT set 6432
Please input SLAVE_1_LOCAL_PORT (备库通信端口)[6434]:
SLAVE_1_LOCAL_PORT set 6434
Please input MASTER_NODENAME [opengauss_master]:
MASTER_NODENAME set opengauss_master
Please input SLAVE_NODENAME [opengauss_slave1]:
SLAVE_NODENAME set opengauss_slave1
Please input openGauss VERSION [1.1.0]: 5.0.0
openGauss VERSION set 5.0.0

# 验证主从
docker exec -it opengauss_master /bin/bash
su - omm
gs_ctl query -D /var/lib/opengauss/data/
[2023-05-16 03:40:13.936][399][][gs_ctl]: gs_ctl query ,datadir is /var/lib/opengauss/data
 HA state:
        local_role                     : Primary
        static_connections             : 1
        db_state                       : Normal
        detail_information             : Normal

 Senders info:
        sender_pid                     : 351
        local_role                     : Primary
        peer_role                      : Standby
        peer_state                     : Normal
        state                          : Streaming
        sender_sent_location           : 0/40001C8
        sender_write_location          : 0/40001C8
        sender_flush_location          : 0/40001C8
        sender_replay_location         : 0/40001C8
        receiver_received_location     : 0/40001C8
        receiver_write_location        : 0/40001C8
        receiver_flush_location        : 0/40001C8
        receiver_replay_location       : 0/40001C8
        sync_percent                   : 100%
        sync_state                     : Sync
        sync_priority                  : 1
        sync_most_available            : On
        channel                        : 172.11.0.101:5434-->172.11.0.102:34310

 Receiver info:
No information
```
