# summarize_wal


```shell
postgres=# show summarize_wal;
 summarize_wal
---------------
 off
(1 row)


postgres=# SELECT * FROM pg_stat_activity WHERE backend_type = 'walsummarizer';
 datid | datname | pid | leader_pid | usesysid | usename | application_name | client_addr | client_hostname | client_port | backend_start | xact_start | query_start | state_change | wait_event_type | wait_event | state | backend_xid | backend_xmin | query_id | query | backend_type
-------+---------+-----+------------+----------+---------+------------------+-------------+-----------------+-------------+---------------+------------+-------------+--------------+-----------------+------------+-------+-------------+--------------+----------+-------+--------------
(0 rows)

<!-- 启用 summarize_wal -->
postgres=# ALTER SYSTEM SET summarize_wal TO on;
ALTER SYSTEM

<!-- 重载配置 -->
postgres=# SELECT pg_reload_conf();
 pg_reload_conf
----------------
 t
(1 row)


postgres=# SELECT * FROM pg_stat_activity WHERE backend_type = 'walsummarizer';
 datid | datname |  pid   | leader_pid | usesysid | usename | application_name | client_addr | client_hostname | client_port |         backend_start         | xact_start | query_start | state_change | wait_event_type |    wait_event    | state | backend_xid | backend_xmin | query_id | query | backend_type
-------+---------+--------+------------+----------+---------+------------------+-------------+-----------------+-------------+-------------------------------+------------+-------------+--------------+-----------------+------------------+-------+-------------+--------------+----------+-------+---------------
       |         | 722424 |            |          |         |                  |             |                 |             | 2024-03-31 10:01:48.616281+08 |            |             |              | Activity        | WalSummarizerWal |       |             |              |          |       | walsummarizer
(1 row)


ls -alh Debug/database/pg_wal/summaries
Permissions Size User  Date Modified Name
.rw-------  1.4k black 31 Mar 10:01   000000010000000009000D080000000009493C98.summary
.rw-------   20k black 31 Mar 10:01   000000010000000009493C980000000011C0ABF8.summary
```
