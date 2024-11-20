# 七种JOIN类型
数据初始化
```sql
MySQL root@127.0.0.1:test> CREATE TABLE `tbl_emp` (
                        ->    `id` INT(11) NOT NULL AUTO_INCREMENT,
                        ->    `name` VARCHAR(30) DEFAULT NULL,
                        ->    `deptID` VARCHAR(40) DEFAULT NULL,
                        ->    PRIMARY KEY (`id`)
                        -> ) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
                        ->
Query OK, 0 rows affected
Time: 0.052s

MySQL root@127.0.0.1:test> CREATE TABLE `tbl_dept` (
                        ->    `id` INT(11) NOT NULL AUTO_INCREMENT,
                        ->    `deptName` VARCHAR(30) DEFAULT NULL,
                        ->    `locAdd` VARCHAR(40) DEFAULT NULL,
                        ->    PRIMARY KEY (`id`)
                        -> ) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
                        ->
Query OK, 0 rows affected
Time: 0.053s

MySQL root@127.0.0.1:test> insert into tbl_dept values(1, 'A', 11),(2, 'B', 12),(3, 'C', 13),(4, 'D', 14),(5, 'F', 15)
Query OK, 5 rows affected
Time: 0.015s

MySQL root@127.0.0.1:test> insert into tbl_emp values(1, 'z', 1),(2, 'y', 1),(3, 'x', 1),(4, 'w', 2),(5, 'v', 2),(6, 'u', 3),(7, 't', 4),(8, 's', 51)
Query OK, 8 rows affected
Time: 0.015s


MySQL root@127.0.0.1:test> select * from tbl_emp
+----+------+--------+
| id | name | deptID |
+----+------+--------+
| 1  | z    | 1      |
| 2  | y    | 1      |
| 3  | x    | 1      |
| 4  | w    | 2      |
| 5  | v    | 2      |
| 6  | u    | 3      |
| 7  | t    | 4      |
| 8  | s    | 51     |
+----+------+--------+


MySQL root@127.0.0.1:test> select * from tbl_dept
+----+----------+--------+
| id | deptName | locAdd |
+----+----------+--------+
| 1  | A        | 11     |
| 2  | B        | 12     |
| 3  | C        | 13     |
| 4  | D        | 14     |
| 5  | F        | 15     |
+----+----------+--------+

```

1. A∩ B 
![[imgs/join.png]]
```sql
MySQL root@127.0.0.1:test> SELECT *
                        -> FROM tbl_emp a
                        -> INNER JOIN tbl_dept b # 共有
                        -> ON a.deptID = b.ID
+----+------+--------+----+----------+--------+
| id | name | deptID | id | deptName | locAdd |
+----+------+--------+----+----------+--------+
| 1  | z    | 1      | 1  | A        | 11     |
| 2  | y    | 1      | 1  | A        | 11     |
| 3  | x    | 1      | 1  | A        | 11     |
| 4  | w    | 2      | 2  | B        | 12     |
| 5  | v    | 2      | 2  | B        | 12     |
| 6  | u    | 3      | 3  | C        | 13     |
| 7  | t    | 4      | 4  | D        | 14     |
+----+------+--------+----+----------+--------+
```

2. A(=A∩ B+A*) 
![[imgs/join.jpeg]]
```sql
MySQL root@127.0.0.1:test> SELECT *
                        -> FROM tbl_emp a
                        -> LEFT JOIN tbl_dept b
                        -> ON a.deptID = b.ID
+----+------+--------+--------+----------+--------+
| id | name | deptID | id     | deptName | locAdd |
+----+------+--------+--------+----------+--------+
| 1  | z    | 1      | 1      | A        | 11     |
| 2  | y    | 1      | 1      | A        | 11     |
| 3  | x    | 1      | 1      | A        | 11     |
| 4  | w    | 2      | 2      | B        | 12     |
| 5  | v    | 2      | 2      | B        | 12     |
| 6  | u    | 3      | 3      | C        | 13     |
| 7  | t    | 4      | 4      | D        | 14     |
| 8  | s    | 51     | <null> | <null>   | <null> |
+----+------+--------+--------+----------+--------+
```

3. B(=A∩ B+B*)
![[imgs/join-1.png]]
```sql
MySQL root@127.0.0.1:test> SELECT *
                        -> FROM tbl_emp a
                        -> RIGHT JOIN tbl_dept b
                        -> ON a.deptID = b.ID
+--------+--------+--------+----+----------+--------+
| id     | name   | deptID | id | deptName | locAdd |
+--------+--------+--------+----+----------+--------+
| 3      | x      | 1      | 1  | A        | 11     |
| 2      | y      | 1      | 1  | A        | 11     |
| 1      | z      | 1      | 1  | A        | 11     |
| 5      | v      | 2      | 2  | B        | 12     |
| 4      | w      | 2      | 2  | B        | 12     |
| 6      | u      | 3      | 3  | C        | 13     |
| 7      | t      | 4      | 4  | D        | 14     |
| <null> | <null> | <null> | 5  | F        | 15     |
+--------+--------+--------+----+----------+--------+
```

4. A*(=A-A∩ B)
![[imgs/join-1.jpeg]]
```sql
MySQL root@127.0.0.1:test> SELECT *
                        -> FROM tbl_emp a
                        -> LEFT JOIN tbl_dept b
                        -> ON a.deptID = b.ID # ON时主表保留
                        -> WHERE b.ID IS NULL # 筛选A表数据
+----+------+--------+--------+----------+--------+
| id | name | deptID | id     | deptName | locAdd |
+----+------+--------+--------+----------+--------+
| 8  | s    | 51     | <null> | <null>   | <null> |
+----+------+--------+--------+----------+--------+
```

5. B*(=B-A∩ B)
![[imgs/join-2.jpeg]]
```sql
MySQL root@127.0.0.1:test> SELECT *
                        -> FROM tbl_emp a
                        -> RIGHT JOIN tbl_dept b
                        -> ON a.deptID = b.ID # ON时主表保留
                        -> WHERE a.deptID IS NULL # 筛选B表数据
+--------+--------+--------+----+----------+--------+
| id     | name   | deptID | id | deptName | locAdd |
+--------+--------+--------+----+----------+--------+
| <null> | <null> | <null> | 5  | F        | 15     |
+--------+--------+--------+----+----------+--------+
```

6. A∪ B
![[imgs/join-3.jpeg]]
```sql
SELECT < select_list > 
FROM TableA A 
FULL OUTER JOIN TableB B ## FULL OUTER 仅oracle支持 
ON A.Key = B.Key

MySQL root@127.0.0.1:test> SELECT *
                        -> FROM tbl_emp a
                        -> LEFT JOIN tbl_dept b
                        -> ON a.deptID = b.ID
                        -> UNION
                        -> SELECT *
                        -> FROM tbl_emp a
                        -> RIGHT JOIN tbl_dept b
                        -> ON a.deptID = b.ID
+--------+--------+--------+--------+----------+--------+
| id     | name   | deptID | id     | deptName | locAdd |
+--------+--------+--------+--------+----------+--------+
| 1      | z      | 1      | 1      | A        | 11     |
| 2      | y      | 1      | 1      | A        | 11     |
| 3      | x      | 1      | 1      | A        | 11     |
| 4      | w      | 2      | 2      | B        | 12     |
| 5      | v      | 2      | 2      | B        | 12     |
| 6      | u      | 3      | 3      | C        | 13     |
| 7      | t      | 4      | 4      | D        | 14     |
| 8      | s      | 51     | <null> | <null>   | <null> |
| <null> | <null> | <null> | 5      | F        | 15     |
+--------+--------+--------+--------+----------+--------+
```

7. A∪ B-A∩ B
![[imgs/join-4.jpeg]]
```sql
SELECT < select_list > 
FROM TableA A 
FULL OUTER JOIN TableB B 
ON A.Key = B.Key 
WHERE A.Key IS NULL OR B.Key IS NULL

MySQL root@127.0.0.1:test> SELECT *
                        -> FROM tbl_emp a
                        -> LEFT JOIN tbl_dept b
                        -> ON a.deptID = b.ID
                        -> UNION
                        -> SELECT *
                        -> FROM tbl_emp a
                        -> RIGHT JOIN tbl_dept b
                        -> ON a.deptID = b.ID
                        -> WHERE a.deptID IS NULL OR b.ID IS NULL
+--------+--------+--------+--------+----------+--------+
| id     | name   | deptID | id     | deptName | locAdd |
+--------+--------+--------+--------+----------+--------+
| 1      | z      | 1      | 1      | A        | 11     |
| 2      | y      | 1      | 1      | A        | 11     |
| 3      | x      | 1      | 1      | A        | 11     |
| 4      | w      | 2      | 2      | B        | 12     |
| 5      | v      | 2      | 2      | B        | 12     |
| 6      | u      | 3      | 3      | C        | 13     |
| 7      | t      | 4      | 4      | D        | 14     |
| 8      | s      | 51     | <null> | <null>   | <null> |
| <null> | <null> | <null> | 5      | F        | 15     |
+--------+--------+--------+--------+----------+--------+
```
