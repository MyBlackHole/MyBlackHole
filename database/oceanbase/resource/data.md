
unit
+----------------------------+----------------------------+----------------+------------------+---------+---------+------------+------------+----------+----------+---------------+---------------------+
| gmt_create                 | gmt_modified               | unit_config_id | name             | max_cpu | min_cpu | max_memory | min_memory | max_iops | min_iops | max_disk_size | max_session_num     |
+----------------------------+----------------------------+----------------+------------------+---------+---------+------------+------------+----------+----------+---------------+---------------------+
| 2023-06-29 10:42:56.105761 | 2023-06-29 10:42:56.105761 | 1              | sys_unit_config  | 5.0     | 2.5     | 5798205849 | 4831838208 | 10000    | 5000     | 320902004736  | 9223372036854775807 |
| 2023-10-25 12:17:48.963335 | 2023-10-25 12:17:48.963335 | 1001           | unit1            | 4.0     | 4.0     | 5368709120 | 5368709120 | 128      | 128      | 10737418240   | 64                  |
| 2023-10-25 19:20:26.981841 | 2023-10-25 19:20:26.981841 | 1002           | config_pool4_xzp | 4.0     | 4.0     | 5368709120 | 5368709120 | 128      | 128      | 536870912     | 64                  |
| 2023-10-26 14:07:58.110438 | 2023-10-26 14:07:58.110438 | 1003           | config_pool3_pyq | 4.0     | 4.0     | 5368709120 | 5368709120 | 128      | 128      | 536870912     | 64                  |
| 2023-10-26 14:13:49.136434 | 2023-10-26 14:13:49.136434 | 1004           | config_pool3_dac | 4.0     | 4.0     | 5368709120 | 5368709120 | 128      | 128      | 536870912     | 64                  |
+----------------------------+----------------------------+----------------+------------------+---------+---------+------------+------------+----------+----------+---------------+---------------------+

pool
+----------------------------+----------------------------+------------------+----------------+------------+----------------+-------------------+-----------+--------------+--------------------+
| gmt_create                 | gmt_modified               | resource_pool_id | name           | unit_count | unit_config_id | zone_list         | tenant_id | replica_type | is_tenant_sys_pool |
+----------------------------+----------------------------+------------------+----------------+------------+----------------+-------------------+-----------+--------------+--------------------+
| 2023-06-29 10:42:56.111034 | 2023-06-29 10:42:56.120421 | 1                | sys_pool       | 1          | 1              | zone1;zone2;zone3 | 1         | 0            | 0                  |
| 2023-10-25 12:20:53.034226 | 2023-10-25 14:35:03.321381 | 1001             | pool1          | 1          | 1001           | zone1             | 1001      | 0            | 0                  |
| 2023-10-25 12:21:00.354408 | 2023-10-25 14:36:48.161916 | 1002             | pool2          | 1          | 1001           | zone2             | 1002      | 0            | 0                  |
| 2023-10-26 14:46:42.401112 | 2023-10-26 14:46:42.401112 | 1011             | pool_pool3_gzz | 1          | 1004           | zone3             | -1        | 0            | 0                  |
+----------------------------+----------------------------+------------------+----------------+------------+----------------+-------------------+-----------+--------------+--------------------+


tenant
+----------------------------+----------------------------+-----------+--------------------+-------------+-------------------+-------------------+--------+----------------+---------------+-----------+-----------------------+---------------------------------------------+---------------------+-------------------+------------------------+-----------------------------+-----------------------+--------------------+------------------+----------------------+---------------+
| gmt_create                 | gmt_modified               | tenant_id | tenant_name        | replica_num | zone_list         | primary_zone      | locked | collation_type | info          | read_only | rewrite_merge_version | locality                                    | logonly_replica_num | previous_locality | storage_format_version | storage_format_work_version | default_tablegroup_id | compatibility_mode | drop_tenant_time | status               | in_recyclebin |
+----------------------------+----------------------------+-----------+--------------------+-------------+-------------------+-------------------+--------+----------------+---------------+-----------+-----------------------+---------------------------------------------+---------------------+-------------------+------------------------+-----------------------------+-----------------------+--------------------+------------------+----------------------+---------------+
| 2023-06-29 10:44:07.433298 | 2023-06-29 10:44:07.433298 | 1         | sys                | -1          | zone1;zone2;zone3 | zone1;zone2;zone3 | 0      | 0              | system tenant | 0         | 0                     | FULL{1}@zone1, FULL{1}@zone2, FULL{1}@zone3 | 0                   |                   | 0                      | 0                           | -1                    | 0                  | -1               | TENANT_STATUS_NORMAL | 0             |
| 2023-10-25 14:36:19.043236 | 2023-10-25 14:36:19.043236 | 1001      | tenant1_1698215702 | -1          | zone1             | RANDOM            | 0      | 0              |               | 0         | 0                     | FULL{1}@zone1                               | 0                   |                   | 0                      | 0                           | -1                    | 0                  | -1               | TENANT_STATUS_NORMAL | 0             |
| 2023-10-25 14:38:04.195953 | 2023-10-25 14:38:04.195953 | 1002      | tenant2_1698215702 | -1          | zone2             | RANDOM            | 0      | 0              |               | 0         | 0                     | FULL{1}@zone2                               | 0                   |                   | 0                      | 0                           | -1                    | 0                  | -1               | TENANT_STATUS_NORMAL | 0             |
+----------------------------+----------------------------+-----------+--------------------+-------------+-------------------+-------------------+--------+----------------+---------------+-----------+-----------------------+---------------------------------------------+---------------------+-------------------+------------------------+-----------------------------+-----------------------+--------------------+------------------+----------------------+---------------+


__all_virtual_server_stat
+----------------+----------+-------+-----------+--------------+----------------------+-------------+--------------+----------------------+--------------+---------------+-----------------------+----------+--------------------+----------------+--------------+--------------------+--------------------+--------------------+-------------+----+------------+-------------------------------------------------------------------------------------------+------------------+---------------------+-----------------------+--------------------+-------------------+-----------+----------------------+--------------+------------------+-----------------+----------------+------------+-------------+-----------------+-------------------+-------------------+--------------+------------------+--------------+------------------+----------------------+
| svr_ip         | svr_port | zone  | cpu_total | cpu_assigned | cpu_assigned_percent | mem_total   | mem_assigned | mem_assigned_percent | disk_total   | disk_assigned | disk_assigned_percent | unit_num | migrating_unit_num | merged_version | leader_count | load               | cpu_weight         | memory_weight      | disk_weight | id | inner_port | build_version                                                                             | register_time    | last_heartbeat_time | block_migrate_in_time | start_service_time | last_offline_time | stop_time | force_stop_heartbeat | admin_status | heartbeat_status | with_rootserver | with_partition | mem_in_use | disk_in_use | clock_deviation | heartbeat_latency | clock_sync_status | cpu_capacity | cpu_max_assigned | mem_capacity | mem_max_assigned | ssl_key_expired_time |
+----------------+----------+-------+-----------+--------------+----------------------+-------------+--------------+----------------------+--------------+---------------+-----------------------+----------+--------------------+----------------+--------------+--------------------+--------------------+--------------------+-------------+----+------------+-------------------------------------------------------------------------------------------+------------------+---------------------+-----------------------+--------------------+-------------------+-----------+----------------------+--------------+------------------+-----------------+----------------+------------+-------------+-----------------+-------------------+-------------------+--------------+------------------+--------------+------------------+----------------------+
| 192.168.80.143 | 2882     | zone1 | 12.0      | 6.5          | 54                   | 19327352832 | 10200547328  | 52                   | 320902004736 | 331639422976  | 103                   | 2        | 0                  | 24             | 1436         | 0.5348124098124097 | 0.5064935064935064 | 0.4935064935064935 | 0.0         | 1  | 2881       | 3.2.3.3_107000092023011911-7699b9a5504eec15ff5ef1f2a95e60136802b170(Jan 19 2023 11:29:19) | 0                | 1698306783946326    | 0                     | 1697424882030617   | 0                 | 0         | 0                    | NORMAL       | alive            | 1               | 1              | 0          | 287309824   | 7               | 349               | SYNC              | 12.0         | 9.0              | 19327352832  | 11166914969      | 0                    |
| 192.168.80.144 | 2882     | zone2 | 12.0      | 6.5          | 54                   | 19327352832 | 10200547328  | 52                   | 320902004736 | 331639422976  | 103                   | 2        | 0                  | 24             | 149          | 0.5348124098124097 | 0.5064935064935064 | 0.4935064935064935 | 0.0         | 2  | 2881       | 3.2.3.3_107000092023011911-7699b9a5504eec15ff5ef1f2a95e60136802b170(Jan 19 2023 11:29:19) | 1697426725713672 | 1698306784153415    | 0                     | 1697424881638323   | 0                 | 0         | 0                    | NORMAL       | alive            | 0               | 1              | 0          | 289406976   | -934            | 484               | SYNC              | 12.0         | 9.0              | 19327352832  | 11166914969      | 0                    |
| 192.168.80.145 | 2882     | zone3 | 12.0      | 6.5          | 54                   | 19327352832 | 10200547328  | 52                   | 320902004736 | 321438875648  | 100                   | 2        | 0                  | 24             | 0            | 0.5348124098124097 | 0.5064935064935064 | 0.4935064935064935 | 0.0         | 3  | 2881       | 3.2.3.3_107000092023011911-7699b9a5504eec15ff5ef1f2a95e60136802b170(Jan 19 2023 11:29:19) | 1697426725713917 | 1698306783407842    | 0                     | 1697424881650543   | 0                 | 0         | 0                    | NORMAL       | alive            | 0               | 1              | 0          | 243269632   | -1540           | 1150              | SYNC              | 12.0         | 9.0              | 19327352832  | 11166914969      | 0                    |
+----------------+----------+-------+-----------+--------------+----------------------+-------------+--------------+----------------------+--------------+---------------+-----------------------+----------+--------------------+----------------+--------------+--------------------+--------------------+--------------------+-------------+----+------------+-------------------------------------------------------------------------------------------+------------------+---------------------+-----------------------+--------------------+-------------------+-----------+----------------------+--------------+------------------+-----------------+----------------+------------+-------------+-----------------+-------------------+-------------------+--------------+------------------+--------------+------------------+----------------------+


+-------+----------+----------------+-----------------+
| zone  | cpu_free | mem_free_gb    | disk_free       |
+-------+----------+----------------+-----------------+
| zone1 | 5.5      | 8.500000000000 | 83.132257461548 |
| zone2 | 5.5      | 8.500000000000 | 83.132257461548 |
| zone3 | 5.5      | 8.500000000000 | 92.632257461548 |
+-------+----------+----------------+-----------------+


+---------+----------------+------------------+------------------+--------------------+-------+-----------+--------------------+----------------+----------+---------------------+-----------------------+---------+---------+------------+------------+----------+----------+---------------+---------------------+
| unit_id | unit_config_id | unit_config_name | resource_pool_id | resource_pool_name | zone  | tenant_id | tenant_name        | svr_ip         | svr_port | migrate_from_svr_ip | migrate_from_svr_port | max_cpu | min_cpu | max_memory | min_memory | max_iops | min_iops | max_disk_size | max_session_num     |
+---------+----------------+------------------+------------------+--------------------+-------+-----------+--------------------+----------------+----------+---------------------+-----------------------+---------+---------+------------+------------+----------+----------+---------------+---------------------+
| 3       | 1              | sys_unit_config  | 1                | sys_pool           | zone3 | 1         | sys                | 192.168.80.145 | 2882     |                     | 0                     | 5.0     | 2.5     | 5798205849 | 4831838208 | 10000    | 5000     | 220902004736  | 9223372036854775807 |
| 2       | 1              | sys_unit_config  | 1                | sys_pool           | zone2 | 1         | sys                | 192.168.80.144 | 2882     |                     | 0                     | 5.0     | 2.5     | 5798205849 | 4831838208 | 10000    | 5000     | 220902004736  | 9223372036854775807 |
| 1       | 1              | sys_unit_config  | 1                | sys_pool           | zone1 | 1         | sys                | 192.168.80.143 | 2882     |                     | 0                     | 5.0     | 2.5     | 5798205849 | 4831838208 | 10000    | 5000     | 220902004736  | 9223372036854775807 |
| 1001    | 1001           | unit1            | 1001             | pool1              | zone1 | 1001      | tenant1_1698215702 | 192.168.80.143 | 2882     |                     | 0                     | 4.0     | 4.0     | 5368709120 | 5368709120 | 128      | 128      | 10737418240   | 64                  |
| 1002    | 1001           | unit1            | 1002             | pool2              | zone2 | 1002      | tenant2_1698215702 | 192.168.80.144 | 2882     |                     | 0                     | 4.0     | 4.0     | 5368709120 | 5368709120 | 128      | 128      | 10737418240   | 64                  |
| 1003    | 1004           | config_pool3_dac | 1011             | pool_pool3_gzz     | zone3 | <null>    | <null>             | 192.168.80.145 | 2882     |                     | 0                     | 4.0     | 4.0     | 5368709120 | 5368709120 | 128      | 128      | 536870912     | 64                  |
+---------+----------------+------------------+------------------+--------------------+-------+-----------+--------------------+----------------+----------+---------------------+-----------------------+---------+---------+------------+------------+----------+----------+---------------+---------------------+


+----------------------------+----------------------------+----------------+----------+----+-------+------------+-----------------+--------+-----------------------+-------------------------------------------------------------------------------------------+-----------+--------------------+--------------+----------------+-------------------+
| gmt_create                 | gmt_modified               | svr_ip         | svr_port | id | zone  | inner_port | with_rootserver | status | block_migrate_in_time | build_version                                                                             | stop_time | start_service_time | first_sessid | with_partition | last_offline_time |
+----------------------------+----------------------------+----------------+----------+----+-------+------------+-----------------+--------+-----------------------+-------------------------------------------------------------------------------------------+-----------+--------------------+--------------+----------------+-------------------+
| 2023-06-29 10:42:43.293958 | 2023-10-16 11:25:25.711551 | 192.168.80.143 | 2882     | 1  | zone1 | 2881       | 1               | active | 0                     | 3.2.3.3_107000092023011911-7699b9a5504eec15ff5ef1f2a95e60136802b170(Jan 19 2023 11:29:19) | 0         | 1697424882030617   | 0            | 1              | 0                 |
| 2023-06-29 10:42:43.817891 | 2023-10-16 11:25:25.713672 | 192.168.80.144 | 2882     | 2  | zone2 | 2881       | 0               | active | 0                     | 3.2.3.3_107000092023011911-7699b9a5504eec15ff5ef1f2a95e60136802b170(Jan 19 2023 11:29:19) | 0         | 1697424881638323   | 0            | 1              | 0                 |
| 2023-06-29 10:42:43.786121 | 2023-10-16 11:25:25.713917 | 192.168.80.145 | 2882     | 3  | zone3 | 2881       | 0               | active | 0                     | 3.2.3.3_107000092023011911-7699b9a5504eec15ff5ef1f2a95e60136802b170(Jan 19 2023 11:29:19) | 0         | 1697424881650543   | 0            | 1              | 0                 |
+----------------------------+----------------------------+----------------+----------+----+-------+------------+-----------------+--------+-----------------------+-------------------------------------------------------------------------------------------+-----------+--------------------+--------------+----------------+-------------------+