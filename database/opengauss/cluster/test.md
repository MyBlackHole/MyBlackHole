
```shell
gs_om -t status --detail

[Ruby@localhost wdg_data]$ gs_om -t stop
Found om task is performing now.Pass this operate.status code is 0
[GAUSS-52503] : Failed to execute checkRunStatus.py. Error: 4

cm_ctl query -Cvdz ALL

AZ3 1  10.6.67.252 10.6.67.252      6001 /opt/cluster/var/lib/engine/data1/data/dn_6001 P Primary Normal | AZ3 2  10.6.67.184 10.6.67.184      6002 /opt/cluster/var/lib/engine/data1/data/dn_6002 S Standby Building(0%) | AZ3 3  10.6.67.185 10.6.67.185      6003 /opt/cluster/var/lib/engine/data1/data/dn_6003 S Standby Normal
AZ3 2  10.6.67.184 10.6.67.184      6004 /opt/cluster/var/lib/engine/data1/data/dn_6004 P Standby Building(0%) | AZ3 1  10.6.67.252 10.6.67.252      6005 /opt/cluster/var/lib/engine/data1/data/dn_6005 S Primary Normal | AZ3 3  10.6.67.185 10.6.67.185      6006 /opt/cluster/var/lib/engine/data1/data/dn_6006 S Standby Normal
AZ3 3  10.6.67.185 10.6.67.185      6007 /opt/cluster/var/lib/engine/data1/data/dn_6007 P Primary Normal | AZ3 2  10.6.67.184 10.6.67.184      6008 /opt/cluster/var/lib/engine/data1/data/dn_6008 S Standby Normal | AZ3 1  10.6.67.252 10.6.67.252      6009 /opt/cluster/var/lib/engine/data1/data/dn_6009 S Standby Normal

cm_ctl stop

cp -rf ~/wdg_data/cn .
cp -rf cn_obk/gaussdb.conf cn
cp -rf cn_obk/gs_hba.conf cn
gs_ctl start -Z coordinator -D /opt/cluster/var/lib/engine/data/cn

[Ruby@localhost data]$ gs_ctl status -Z coordinator -D /opt/cluster/var/lib/engine/data/cn
[2025-07-23 03:12:18.552][1184171][][gs_ctl]: gs_ctl status,datadir is /opt/cluster/var/lib/engine/data/cn
gs_ctl: server is running (PID: 1171607)
/opt/cluster/usr/local/core/app/bin/gaussdb "--coordinator" "-D" "/opt/cluster/var/lib/engine/data/cn"

# 需要连接 gtm 所以无法直接访问
gsql -p8000 -Uroot -dpostgres -W 'xxxxxxxx'
gsql -p8000 -UrdsAdmin -dpostgres




10.6.67.252 dn_6001 dn_6005 dn_6009
mv dn_6004  dn_6005
mv dn_6007  dn_6009
cp -rf dn_obk/dn_6001/gaussdb.conf dn_6001
cp -rf dn_obk/dn_6005/gaussdb.conf dn_6005
cp -rf dn_obk/dn_6009/gaussdb.conf dn_6009
cp -rf dn_obk/dn_6001/gs_hba.conf dn_6001
cp -rf dn_obk/dn_6005/gs_hba.conf dn_6005
cp -rf dn_obk/dn_6009/gs_hba.conf dn_6009
cp ~/recovery.conf dn_6001/
cp ~/recovery.conf dn_6005/
cp ~/recovery.conf dn_6009/

gs_ctl start -Z datanode -D /opt/cluster/var/lib/engine/data1/data/dn_6001
gs_ctl start -Z datanode -D /opt/cluster/var/lib/engine/data1/data/dn_6005
gs_ctl start -Z datanode -D /opt/cluster/var/lib/engine/data1/data/dn_6009

gs_ctl stop -Z datanode -D /opt/cluster/var/lib/engine/data1/data/dn_6001
gs_ctl stop -Z datanode -D /opt/cluster/var/lib/engine/data1/data/dn_6005
gs_ctl stop -Z datanode -D /opt/cluster/var/lib/engine/data1/data/dn_6009


10.6.67.184 dn_6002 dn_6004 dn_6008
mv dn_6001  dn_6002
mv dn_6007  dn_6008
cp -rf dn_obk/dn_6002/gaussdb.conf dn_6002
cp -rf dn_obk/dn_6004/gaussdb.conf dn_6004
cp -rf dn_obk/dn_6008/gaussdb.conf dn_6008
cp -rf dn_obk/dn_6002/gs_hba.conf dn_6002
cp -rf dn_obk/dn_6004/gs_hba.conf dn_6004
cp -rf dn_obk/dn_6008/gs_hba.conf dn_6008
gs_ctl start -Z datanode -D /opt/cluster/var/lib/engine/data1/data/dn_6002
gs_ctl start -Z datanode -D /opt/cluster/var/lib/engine/data1/data/dn_6004
gs_ctl start -Z datanode -D /opt/cluster/var/lib/engine/data1/data/dn_6008

gs_ctl stop -Z datanode -D /opt/cluster/var/lib/engine/data1/data/dn_6002
gs_ctl stop -Z datanode -D /opt/cluster/var/lib/engine/data1/data/dn_6004
gs_ctl stop -Z datanode -D /opt/cluster/var/lib/engine/data1/data/dn_6008

/opt/cluster/var/lib/log/Ruby/gs_log/dn_6002
cp ~/recovery.conf dn_6002/
cp ~/recovery.conf dn_6004/
cp ~/recovery.conf dn_6008/


10.6.67.185 dn_6003 dn_6006 dn_6007
mv dn_6001  dn_6003
mv dn_6004  dn_6006
cp -rf dn_obk/dn_6003/gaussdb.conf dn_6003
cp -rf dn_obk/dn_6006/gaussdb.conf dn_6006
cp -rf dn_obk/dn_6007/gaussdb.conf dn_6007
cp -rf dn_obk/dn_6003/gs_hba.conf dn_6003
cp -rf dn_obk/dn_6006/gs_hba.conf dn_6006
cp -rf dn_obk/dn_6007/gs_hba.conf dn_6007

cp ~/recovery.conf dn_6003/
cp ~/recovery.conf dn_6006/
cp ~/recovery.conf dn_6007/

gs_ctl start -Z datanode -D /opt/cluster/var/lib/engine/data1/data/dn_6003
gs_ctl start -Z datanode -D /opt/cluster/var/lib/engine/data1/data/dn_6006
gs_ctl start -Z datanode -D /opt/cluster/var/lib/engine/data1/data/dn_6007

gs_ctl stop -Z datanode -D /opt/cluster/var/lib/engine/data1/data/dn_6003
gs_ctl stop -Z datanode -D /opt/cluster/var/lib/engine/data1/data/dn_6006
gs_ctl stop -Z datanode -D /opt/cluster/var/lib/engine/data1/data/dn_6007



dn_6001 dn_6002 dn_6003
dn_6004 dn_6005 dn_6006
dn_6007 dn_6008 dn_6009

/opt/cluster/var/lib/engine/data1/data/dn_6001
/opt/cluster/var/lib/engine/data1/data/dn_6004
/opt/cluster/var/lib/engine/data1/data/dn_6007


/opt/cluster/var/lib/engine/data1/data/dn_6002
/opt/cluster/var/lib/engine/data1/data/dn_6005
/opt/cluster/var/lib/engine/data1/data/dn_6008

/opt/cluster/var/lib/engine/data1/data/dn_6003
/opt/cluster/var/lib/engine/data1/data/dn_6006
/opt/cluster/var/lib/engine/data1/data/dn_6009


mkdir dn_obk
mv dn_6* dn_obk/
cp -rf ~/wdg_data/dn/* .

cp cm_obk/ cm

cp -rf ~/wdg_data/gtm/ .

cp gtm_obk/gtm.conf gtm

cm_ctl start




gaussdb=> select * from pgxc_node;
     node_name     | node_type | node_port |  node_host  | node_port1 | node_host1  | hostis_primary | nodeis_primary | nodeis_preferred |   node_id   | sctp_port | control_port | sctp_port1 | control_port1 | nodeis_central | nodeis_acti
ve
-------------------+-----------+-----------+-------------+------------+-------------+----------------+----------------+------------------+-------------+-----------+--------------+------------+---------------+----------------+------------
---
 cn_5001           | C         |      8000 | 10.6.67.129 |       8000 | 10.6.67.129 | t              | f              | f                |  1120683504 |      8002 |         8003 |          0 |             0 | t              | t
 cn_5002           | C         |      8000 | 10.6.67.130 |       8000 | 10.6.67.130 | t              | f              | f                | -1736975100 |      8002 |         8003 |          0 |             0 | f              | t
 cn_5003           | C         |      8000 | 10.6.67.131 |       8000 | 10.6.67.131 | t              | f              | f                |  -125853378 |      8002 |         8003 |          0 |             0 | f              | t
 dn_6004_6005_6006 | S         |     40020 | 10.6.67.130 |      40020 | 10.6.67.130 | t              | f              | f                |  -564789568 |     40022 |        40023 |          0 |             0 | f              | t
 dn_6004_6005_6006 | S         |     40020 | 10.6.67.129 |      40020 | 10.6.67.129 | t              | f              | f                |  -564789567 |     40022 |        40023 |          0 |             0 | f              | t
 dn_6004_6005_6006 | S         |     40020 | 10.6.67.131 |      40020 | 10.6.67.131 | t              | f              | f                |  -564789566 |     40022 |        40023 |          0 |             0 | f              | t
 dn_6001_6002_6003 | S         |     40000 | 10.6.67.129 |      40000 | 10.6.67.129 | t              | f              | f                | -1072999043 |     40002 |        40003 |          0 |             0 | f              | t
 dn_6001_6002_6003 | S         |     40000 | 10.6.67.130 |      40000 | 10.6.67.130 | t              | f              | f                | -1072999042 |     40002 |        40003 |          0 |             0 | f              | t
 dn_6001_6002_6003 | S         |     40000 | 10.6.67.131 |      40000 | 10.6.67.131 | t              | f              | f                | -1072999041 |     40002 |        40003 |          0 |             0 | f              | t
 dn_6007_6008_6009 | S         |     40040 | 10.6.67.131 |      40040 | 10.6.67.131 | t              | f              | f                |  1532339558 |     40042 |        40043 |          0 |             0 | f              | t
 dn_6007_6008_6009 | S         |     40040 | 10.6.67.130 |      40040 | 10.6.67.130 | t              | f              | f                |  1532339559 |     40042 |        40043 |          0 |             0 | f              | t
 dn_6007_6008_6009 | S         |     40040 | 10.6.67.129 |      40040 | 10.6.67.129 | t              | f              | f                |  1532339560 |     40042 |        40043 |          0 |             0 | f              | t
(12 rows)



AZ3 1  10.6.67.252 6001 /opt/cluster/var/lib/engine/data1/data/dn_6001 P Standby Normal | AZ3 2  10.6.67.184 6002 /opt/cluster/var/lib/engine/data1/data/dn_6002 S Primary Normal | AZ3 3  10.6.67.185 6003 /opt/cluster/var/lib/engine/data1/data/dn_6003 S Standby Normal
AZ3 2  10.6.67.184 6004 /opt/cluster/var/lib/engine/data1/data/dn_6004 P Standby Normal | AZ3 1  10.6.67.252 6005 /opt/cluster/var/lib/engine/data1/data/dn_6005 S Primary Normal | AZ3 3  10.6.67.185 6006 /opt/cluster/var/lib/engine/data1/data/dn_6006 S Standby Normal
AZ3 3  10.6.67.185 6007 /opt/cluster/var/lib/engine/data1/data/dn_6007 P Standby Normal | AZ3 2  10.6.67.184 6008 /opt/cluster/var/lib/engine/data1/data/dn_6008 S Primary Normal | AZ3 1  10.6.67.252 6009 /opt/cluster/var/lib/engine/data1/data/dn_6009 S Standby Normal


AZ3 1  10.6.67.252 5001 /opt/cluster/var/lib/engine/data/cn Normal
AZ3 2  10.6.67.184 5002 /opt/cluster/var/lib/engine/data/cn Normal
AZ3 3  10.6.67.185 5003 /opt/cluster/var/lib/engine/data/cn Normal

gsql -p8000 -UrdsAdmin -dpostgres

alter node cn_5001 with(host='10.6.67.252',host1='10.6.67.252');
alter node cn_5002 with(host='10.6.67.184',host1='10.6.67.184');
alter node cn_5003 with(host='10.6.67.185',host1='10.6.67.185');

update pgxc_node set node_host='10.6.67.252',node_host1='10.6.67.252' where node_host='10.6.67.129';
update pgxc_node set node_host='10.6.67.184',node_host1='10.6.67.184' where node_host='10.6.67.130';
update pgxc_node set node_host='10.6.67.185',node_host1='10.6.67.185' where node_host='10.6.67.131';
```
