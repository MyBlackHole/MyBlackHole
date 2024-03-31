# config


## pg_stat_activity
```shell
postgres=# SELECT * FROM pg_stat_activity;
 datid | datname  |  pid   | leader_pid | usesysid | usename | application_name | client_addr | client_hostname | client_port |         backend_start         |          xact_start           |          query_start          |         state_change         | wait_event_type |     wait_event      | state  | backend_xid | backend_xmin | query_id |              query
           |         backend_type
-------+----------+--------+------------+----------+---------+------------------+-------------+-----------------+-------------+-------------------------------+-------------------------------+-------------------------------+------------------------------+-----------------+---------------------+--------+-------------+--------------+----------+----------------------
-----------+------------------------------
       |          | 686533 |            |          |         |                  |             |                 |             | 2024-03-30 20:03:56.361549+08 |                               |                               |                              | Activity        | AutovacuumMain      |        |             |              |          |
           | autovacuum launcher
       |          | 686534 |            |       10 | black   |                  |             |                 |             | 2024-03-30 20:03:56.362034+08 |                               |                               |                              | Activity        | LogicalLauncherMain |        |             |              |          |
           | logical replication launcher
     5 | postgres | 722935 |            |       10 | black   | psql             |             |                 |          -1 | 2024-03-31 10:03:02.478682+08 | 2024-03-31 10:03:19.479377+08 | 2024-03-31 10:03:19.479377+08 | 2024-03-31 10:03:19.47938+08 |                 |                     | active |             |          803 |          | SELECT * FROM pg_stat
_activity; | client backend
       |          | 686530 |            |          |         |                  |             |                 |             | 2024-03-30 20:03:56.34751+08  |                               |                               |                              | Activity        | BgwriterHibernate   |        |             |              |          |
           | background writer
       |          | 686529 |            |          |         |                  |             |                 |             | 2024-03-30 20:03:56.347173+08 |                               |                               |                              | Activity        | CheckpointerMain    |        |             |              |          |
           | checkpointer
       |          | 686532 |            |          |         |                  |             |                 |             | 2024-03-30 20:03:56.361274+08 |                               |                               |                              | Activity        | WalWriterMain       |        |             |              |          |
           | walwriter
       |          | 722424 |            |          |         |                  |             |                 |             | 2024-03-31 10:01:48.616281+08 |                               |                               |                              | Activity        | WalSummarizerWal    |        |             |              |          |
           | walsummarizer
(7 rows)
```

