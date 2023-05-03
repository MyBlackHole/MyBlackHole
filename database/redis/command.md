- zrevrangebyscore
ZREVRANGEBYSCORE key max min [WITHSCORES] [LIMIT offset count]
```redis
127.0.0.1:6379> ZADD salary 10086 jack
(integer) 1
127.0.0.1:6379> ZADD salary 5000 tom
(integer) 1
127.0.0.1:6379> ZADD salary 3500 joe
(integer) 1
127.0.0.1:6379> ZREVRANGEBYSCORE salary +inf -inf # 逆序排列所有成员
1) "jack"
2) "tom"
3) "joe"
127.0.0.1:6379> ZREVRANGEBYSCORE salary 10000 2000  # 逆序排列薪水介于 10000 和 2000 之间的成员
1) "tom"
2) "joe"
127.0.0.1:6379>
```
