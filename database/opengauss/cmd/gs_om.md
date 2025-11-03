# gs_om


```shell
[root@localhost ~]$ su - Ruby 
[Ruby@localhost ~]$ source gauss_env_file
[Ruby@localhost ~]$ gs_om -t status --detail
[  CMServer State   ]

node               node_ip         instance                                           state
---------------------------------------------------------------------------------------------
AZ3 1  10.6.67.252 10.6.67.252      1    /opt/cluster/var/lib/engine/data/cm/cm_server Primary
AZ3 2  10.6.67.184 10.6.67.184      2    /opt/cluster/var/lib/engine/data/cm/cm_server Standby
AZ3 3  10.6.67.185 10.6.67.185      3    /opt/cluster/var/lib/engine/data/cm/cm_server Standby

[    ETCD State     ]

node               node_ip         instance                         state
---------------------------------------------------------------------------------
AZ3 1  10.6.67.252 10.6.67.252      7001 /opt/cluster/usr/local/etcd StateFollower
AZ3 2  10.6.67.184 10.6.67.184      7002 /opt/cluster/usr/local/etcd StateFollower
AZ3 3  10.6.67.185 10.6.67.185      7003 /opt/cluster/usr/local/etcd StateLeader

[   Cluster State   ]

cluster_state   : Degraded
redistributing  : No
balanced        : No
current_az      : AZ_ALL

[ Coordinator State ]

node               node_ip         instance                                 state
-----------------------------------------------------------------------------------
AZ3 1  10.6.67.252 10.6.67.252      5001 /opt/cluster/var/lib/engine/data/cn Normal
AZ3 2  10.6.67.184 10.6.67.184      5002 /opt/cluster/var/lib/engine/data/cn Normal
AZ3 3  10.6.67.185 10.6.67.185      5003 /opt/cluster/var/lib/engine/data/cn Normal

[ Central Coordinator State ]

node               node_ip         instance                                 state
-----------------------------------------------------------------------------------
AZ3 3  10.6.67.185 10.6.67.185      5003 /opt/cluster/var/lib/engine/data/cn Normal

[     GTM State     ]

node               node_ip         instance                                  state                    sync_state
--------------------------------------------------------------------------------------------------------------------
AZ3 1  10.6.67.252 10.6.67.252      1002 /opt/cluster/var/lib/engine/data/gtm S Standby Connection ok  Sync
AZ3 2  10.6.67.184 10.6.67.184      1001 /opt/cluster/var/lib/engine/data/gtm P Standby Connection ok  Sync
AZ3 3  10.6.67.185 10.6.67.185      1003 /opt/cluster/var/lib/engine/data/gtm S Primary Connection ok  Sync

[  Datanode State   ]

node               node_ip         instance                                            state            | node               node_ip         instance                                            state            | node               node_ip         instance                                            state
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
AZ3 1  10.6.67.252 10.6.67.252      6001 /opt/cluster/var/lib/engine/data1/data/dn_6001 P Primary Normal | AZ3 2  10.6.67.184 10.6.67.184      6002 /opt/cluster/var/lib/engine/data1/data/dn_6002 S Standby Building(0%) | AZ3 3  10.6.67.185 10.6.67.185      6003 /opt/cluster/var/lib/engine/data1/data/dn_6003 S Standby Normal
AZ3 2  10.6.67.184 10.6.67.184      6004 /opt/cluster/var/lib/engine/data1/data/dn_6004 P Standby Building(0%) | AZ3 1  10.6.67.252 10.6.67.252      6005 /opt/cluster/var/lib/engine/data1/data/dn_6005 S Primary Normal | AZ3 3  10.6.67.185 10.6.67.185      6006 /opt/cluster/var/lib/engine/data1/data/dn_6006 S Standby Normal
AZ3 3  10.6.67.185 10.6.67.185      6007 /opt/cluster/var/lib/engine/data1/data/dn_6007 P Primary Normal | AZ3 2  10.6.67.184 10.6.67.184      6008 /opt/cluster/var/lib/engine/data1/data/dn_6008 S Standby Normal | AZ3 1  10.6.67.252 10.6.67.252      6009 /opt/cluster/var/lib/engine/data1/data/dn_6009 S Standby Normal


```
