# toast

The Oversized-Attribute Storage Technique (超尺寸属性存储技术)

```shell
black@127:postgres> create table blog(id int, title text, content text);
CREATE TABLE
Time: 0.016s
black@127:postgres> \d+ blog;
+---------+---------+-----------+----------+--------------+-------------+
| Column  | Type    | Modifiers | Storage  | Stats target | Description |
|---------+---------+-----------+----------+--------------+-------------|
| id      | integer |           | plain    | <null>       | <null>      |
| title   | text    |           | extended | <null>       | <null>      |
| content | text    |           | external | <null>       | <null>      |
+---------+---------+-----------+----------+--------------+-------------+
Time: 0.013s

ALTER TABLE blog ALTER content SET STORAGE EXTERNAL; 

black@127:postgres> select relname,relfilenode,reltoastrelid from pg_class where relname='blog';
+---------+-------------+---------------+
| relname | relfilenode | reltoastrelid |
|---------+-------------+---------------|
| blog    | 16715       | 16718         |
+---------+-------------+---------------+
SELECT 1
Time: 0.006s
<!-- blog 表的 oid 为16441，其对应 TOAST 表的 oid 为 16718 -->

black@127:postgres> \d pg_toast.pg_toast_16715;
+------------+---------+
| Column     | Type    |
|------------+---------|
| chunk_id   | oid     |
| chunk_seq  | integer |
| chunk_data | bytea   |
+------------+---------+
Time: 0.011s


black@127:postgres> select relname,relfilenode,reltoastrelid from pg_class where relname='pg_toast_16715';
+----------------+-------------+---------------+
| relname        | relfilenode | reltoastrelid |
|----------------+-------------+---------------|
| pg_toast_16715 | 16718       | 0             |
+----------------+-------------+---------------+
SELECT 1
Time: 0.008s
```
