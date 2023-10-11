# 

## 
- CLIENT LIST
查询连接
```shell
127.0.0.1:6379[1]> CLIENT LIST
id=2860896 addr=192.168.110.151:51452 fd=37 name= age=1281 idle=1252 flags=N db=1 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=0 obl=0 oll=0 omem=0 events=r cmd=lpush
id=2860897 addr=192.168.110.151:51454 fd=38 name= age=1281 idle=0 flags=N db=2 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=0 obl=0 oll=0 omem=0 events=r cmd=mget
id=2860823 addr=192.168.110.152:47450 fd=31 name= age=1315 idle=1315 flags=N db=1 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=0 obl=0 oll=0 omem=0 events=r cmd=sadd
```
## zset
- ZCARD
ZCARD key
获取成员数量
```shell
192.168.78.212:6379[1]> ZCARD unacked_index
(integer) 1
```

- ZCOUNT
ZCOUNT key min max
指定分数范围的成员数量
```shell
192.168.78.212:6379[1]> ZCOUNT unacked_index -inf +inf
(integer) 1
```

- ZSCORE
ZSCORE key member
返回成员 member 的 score 值
```shell
192.168.78.212:6379[1]> ZSCORE unacked_index 72ed1902-5cfd-4da8-be16-9d570d6b4b0e
"1684804923.4498105"
```

- zrange
ZRANGE key start stop [WITHSCORES](显示对应的score)
便利 start stop 范围数据
```shell
127.0.0.1:6379[1]> ZRANGE unacked_index 0 -1
1) "cbd85c98-6a5b-4fdd-bebd-7c13615840dc"
2) "b175c6a1-fc28-4a42-8cb6-e44a36558db4"
3) "dabfac80-2f41-4cd4-bb39-2b99d477341e"
4) "9de07696-60f3-4676-b90b-b109f0e0025a"


192.168.78.212:6379[1]> ZRANGE unacked_index 0 -1 withscores
1) "72ed1902-5cfd-4da8-be16-9d570d6b4b0e"
2) "1684804923.4498105"
```

- zrevrangebyscore
获取 zset 指定范围的成员
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
## hash
- hgetall
获取 hash 所有 key alue
