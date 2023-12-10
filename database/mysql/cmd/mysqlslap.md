# mysqlslap

## 参数
--defaults-file，配置文件存放位置
--create-schema，测试的schema，MySQL中schema也就是database
--concurrency，并发数
--engines，测试引擎，可以有多个，用分隔符隔开。
--iterations，迭代的实验次数
--socket，socket，文件位置
--debug-info，打印内存和CPU的信息
--only-print，只打印测试语句而不实际执行
--auto-generate-sql，自动产生测试SQL
--auto-generate-sql-load-type，测试SQL的类型。类型有mixed，update，write，key，read。
--number-of-queries，执行的SQL总数量
--number-int-cols，表内int列的数量--number-char-cols，表内char列的数量
--query=name，使用自定义脚本执行测试

例如可以调用自定义的一个存储过程或者sql语句来执行测试。

## 例子
- 执行插入
```shell
mysqlslap -h 192.168.90.207 -P 8888 -u test -pP@ssw0rd --concurrency=10 --number-of-queries=1 --create-schema=UDALTEST --query="insert into user (id, score, name) values(uuid(), RAND() * 100, 'test1');"
mysqlslap -h 192.168.90.207 -P 8888 -u test -pP@ssw0rd --concurrency=10 --number-of-queries=1 --create-schema=UDALTEST --query="insert into user (score, name) values(RAND() * 100, 'test1');"

mysqlslap -h 192.168.90.207 -P 8888 -u test -pP@ssw0rd --concurrency=10 --number-of-queries=1 --create-schema=UDALTEST --query="UDAL XA START;insert into user (score, name) values(RAND() * 100, 'test1');insert into user (score, name) values(RAND() * 100, 'test1');COMMIT;"
```
