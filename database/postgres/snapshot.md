# snapshot 快照

## 例子
```
black@127:black> create table test(id int);
CREATE TABLE
Time: 0.014s
black@127:black> insert into test values (1),(2);
INSERT 0 2
Time: 0.010s

# session 1
black@127:black> begin TRANSACTION ISOLATION LEVEL repeatable read;
BEGIN
Time: 0.001s

black@127:black> select * from pg_export_snapshot();
+---------------------+
| pg_export_snapshot  |
|---------------------|
| 00000004-0000004B-1 |
+---------------------+
SELECT 1
Time: 0.021s

black@127:black> insert into test values (3);
INSERT 0 1
Time: 0.002s

black@127:black> select * from pg_export_snapshot();
+---------------------+
| pg_export_snapshot  |
|---------------------|
| 00000004-0000004B-3 |
+---------------------+
SELECT 1
Time: 0.011s

black@127:black> select * from txid_current();
+--------------+
| txid_current |
|--------------|
| 741          |
+--------------+
SELECT 1
Time: 0.011s

black@127:black> select * from txid_current_snapshot();
+-----------------------+
| txid_current_snapshot |
|-----------------------|
| 741:741:              |
+-----------------------+
SELECT 1
Time: 0.005s

# session 2
black@127:black> insert into test values (5);
INSERT 0 1
Time: 0.004s

# session 3
black@127:black> select * from test;
+----+
| id |
|----|
| 1  |
| 2  |
| 5  |
+----+
SELECT 3
Time: 0.021s

# session 4
black@127:black> begin TRANSACTION ISOLATION LEVEL repeatable read;
BEGIN
Time: 0.001s

black@127:black> SET TRANSACTION SNAPSHOT '00000004-0000004B-3';
SET
Time: 0.001s

black@127:black> select * from test;
+----+
| id |
|----|
| 1  |
| 2  |
+----+
SELECT 2
Time: 0.007s

black@127:black> select * from txid_current();
+--------------+
| txid_current |
|--------------|
| 743          |
+--------------+
SELECT 1
Time: 0.004s

black@127:black> select * from txid_current_snapshot();
+-----------------------+
| txid_current_snapshot |
|-----------------------|
| 741:741:              |
+-----------------------+
SELECT 1
Time: 0.005s
```
