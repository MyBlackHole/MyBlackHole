# cluster



```shell
<!-- ob 集群节点信息查询 -->
select * from oceanbase.__all_server;

+----------------------------+----------------------------+----------------+----------+----+-------+------------+-----------------+--------+-----------------------+-------------------------------------------------------------------------------------------+-----------+--------------------+--------------+----------------+-------------------+
| gmt_create                 | gmt_modified               | svr_ip         | svr_port | id | zone  | inner_port | with_rootserver | status | block_migrate_in_time | build_version                                                                             | stop_time | start_service_time | first_sessid | with_partition | last_offline_time |
+----------------------------+----------------------------+----------------+----------+----+-------+------------+-----------------+--------+-----------------------+-------------------------------------------------------------------------------------------+-----------+--------------------+--------------+----------------+-------------------+
| 2023-06-29 10:03:38.306227 | 2023-10-11 16:11:04.659877 | 192.168.80.140 |     2882 |  1 | zone1 |       2881 |               1 | active |                     0 | 3.2.3.3_107000092023011911-7699b9a5504eec15ff5ef1f2a95e60136802b170(Jan 19 2023 11:29:19) |         0 |   1696930386213580 |            0 |              1 |                 0 |
| 2023-06-29 10:03:37.317072 | 2023-10-11 16:11:04.660461 | 192.168.80.141 |     2882 |  2 | zone2 |       2881 |               0 | active |                     0 | 3.2.3.3_107000092023011911-7699b9a5504eec15ff5ef1f2a95e60136802b170(Jan 19 2023 11:29:19) |         0 |   1696647270700648 |            0 |              1 |                 0 |
| 2023-06-29 10:03:38.315813 | 2023-10-11 16:11:04.660745 | 192.168.80.142 |     2882 |  3 | zone3 |       2881 |               0 | active |                     0 | 3.2.3.3_107000092023011911-7699b9a5504eec15ff5ef1f2a95e60136802b170(Jan 19 2023 11:29:19) |         0 |   1696647266764415 |            0 |              1 |                 0 |
+----------------------------+----------------------------+----------------+----------+----+-------+------------+-----------------+--------+-----------------------+-------------------------------------------------------------------------------------------+-----------+--------------------+--------------+----------------+-------------------+
3 rows in set (0.00 sec)
```

```shell
obclient [oceanbase]> show parameters like "cluster";
+-------+----------+----------------+----------+---------+-----------+----------------+---------------------+----------+---------+---------+-------------------+
| zone  | svr_type | svr_ip         | svr_port | name    | data_type | value          | info                | section  | scope   | source  | edit_level        |
+-------+----------+----------------+----------+---------+-----------+----------------+---------------------+----------+---------+---------+-------------------+
| zone1 | observer | 192.168.80.140 |     2882 | cluster | NULL      | wdg80140141142 | Name of the cluster | OBSERVER | CLUSTER | DEFAULT | DYNAMIC_EFFECTIVE |
| zone2 | observer | 192.168.80.141 |     2882 | cluster | NULL      | wdg80140141142 | Name of the cluster | OBSERVER | CLUSTER | DEFAULT | DYNAMIC_EFFECTIVE |
| zone3 | observer | 192.168.80.142 |     2882 | cluster | NULL      | wdg80140141142 | Name of the cluster | OBSERVER | CLUSTER | DEFAULT | DYNAMIC_EFFECTIVE |
+-------+----------+----------------+----------+---------+-----------+----------------+---------------------+----------+---------+---------+-------------------+
3 rows in set (0.025 sec)
```