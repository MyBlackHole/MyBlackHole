# tenant


- 创建租户
```shell
CREATE RESOURCE UNIT S1_unit_config
                MEMORY_SIZE = '5G',
                MAX_CPU = 1, MIN_CPU = 1,
                LOG_DISK_SIZE = '6G',
                MAX_IOPS = 10000, MIN_IOPS = 10000, IOPS_WEIGHT=1;

SELECT * FROM oceanbase.DBA_OB_UNIT_CONFIGS WHERE NAME = 'S1_unit_config';

+----------------+----------------+----------------------------+----------------------------+---------+---------+-------------+---------------+----------+----------+-------------+
| UNIT_CONFIG_ID | NAME           | CREATE_TIME                | MODIFY_TIME                | MAX_CPU | MIN_CPU | MEMORY_SIZE | LOG_DISK_SIZE | MAX_IOPS | MIN_IOPS | IOPS_WEIGHT |
+----------------+----------------+----------------------------+----------------------------+---------+---------+-------------+---------------+----------+----------+-------------+
| 1001           | S1_unit_config | 2024-08-29 12:01:48.497818 | 2024-08-29 12:01:48.497818 | 1.0     | 1.0     | 3221225472  | 6442450944    | 10000    | 10000    | 1           |
+----------------+----------------+----------------------------+----------------------------+---------+---------+-------------+---------------+----------+----------+-------------+


CREATE RESOURCE POOL mq_pool_01
                UNIT='S1_unit_config',
                UNIT_NUM=1,
                ZONE_LIST=('zone1');


SELECT * FROM oceanbase.DBA_OB_RESOURCE_POOLS WHERE NAME ='mq_pool_01';

+------------------+------------+-----------+----------------------------+----------------------------+------------+----------------+-----------+--------------+
| RESOURCE_POOL_ID | NAME       | TENANT_ID | CREATE_TIME                | MODIFY_TIME                | UNIT_COUNT | UNIT_CONFIG_ID | ZONE_LIST | REPLICA_TYPE |
+------------------+------------+-----------+----------------------------+----------------------------+------------+----------------+-----------+--------------+
| 1                | sys_pool   | 1         | 2024-08-28 15:09:50.159771 | 2024-08-28 15:09:50.184044 | 1          | 1              | zone1     | FULL         |
| 1001             | mq_pool_01 | <null>    | 2024-08-29 12:04:33.432708 | 2024-08-29 12:04:33.432708 | 1          | 1001           | zone1     | FULL         |
+------------------+------------+-----------+----------------------------+----------------------------+------------+----------------+-----------+--------------+


CREATE TENANT IF NOT EXISTS mq_t1 
                PRIMARY_ZONE='zone1', 
                RESOURCE_POOL_LIST=('mq_pool_01')
                set OB_TCP_INVITED_NODES='%';

SELECT * FROM DBA_OB_TENANTS WHERE TENANT_NAME = 'mq_t1';
+-----------+-------------+-------------+----------------------------+----------------------------+--------------+---------------+-------------------+--------------------+--------+---------------+--------+-------------+-------------------+------------------+---------------------+---------------------+---------------------+---------------------+--------------+----------------------------+----------+------------+-----------+
| TENANT_ID | TENANT_NAME | TENANT_TYPE | CREATE_TIME                | MODIFY_TIME                | PRIMARY_ZONE | LOCALITY      | PREVIOUS_LOCALITY | COMPATIBILITY_MODE | STATUS | IN_RECYCLEBIN | LOCKED | TENANT_ROLE | SWITCHOVER_STATUS | SWITCHOVER_EPOCH | SYNC_SCN            | REPLAYABLE_SCN      | READABLE_SCN        | RECOVERY_UNTIL_SCN  | LOG_MODE     | ARBITRATION_SERVICE_STATUS | UNIT_NUM | COMPATIBLE | MAX_LS_ID |
+-----------+-------------+-------------+----------------------------+----------------------------+--------------+---------------+-------------------+--------------------+--------+---------------+--------+-------------+-------------------+------------------+---------------------+---------------------+---------------------+---------------------+--------------+----------------------------+----------+------------+-----------+
| 1002      | mq_t1       | USER        | 2024-08-29 12:06:08.115006 | 2024-08-29 12:07:01.157927 | zone1        | FULL{1}@zone1 | <null>            | MYSQL              | NORMAL | NO            | NO     | PRIMARY     | NORMAL            | 0                | 1724904469637717163 | 1724904469637717163 | 1724904469637717162 | 4611686018427387903 | NOARCHIVELOG | DISABLED                   | 1        | 4.2.1.9    | 1001      |
+-----------+-------------+-------------+----------------------------+----------------------------+--------------+---------------+-------------------+--------------------+--------+---------------+--------+-------------+-------------------+------------------+---------------------+---------------------+---------------------+---------------------+--------------+----------------------------+----------+------------+-----------+

obclient -h172.30.xx.xx -P2883 -uroot@mq_t1#cluster  -A

ALTER USER root IDENTIFIED BY '****';
Query OK, 0 rows affected


DROP TENANT mq_t1;

<!--恢复租户-->
FLASHBACK TENANT mq_t1 TO BEFORE DROP;
```