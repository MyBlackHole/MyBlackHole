# pg_control

- 读取 pg_control
```shell
template1> select pg_control_checkpoint()
+-----------------------------------------------------------------------------------------------------------------+
| pg_control_checkpoint                                                                                           |
|-----------------------------------------------------------------------------------------------------------------|
| (0/17025E0,0/17025A8,000000010000000000000001,1,1,t,0:737,24577,1,0,726,1,737,1,1,0,0,"2023-03-30 09:37:05+08") |
+-----------------------------------------------------------------------------------------------------------------+
SELECT 1
Time: 0.012s

# $PGDATA/global/pg_control
sudo ./pg_controldata --pgdata /var/lib/postgresql/14/main
```