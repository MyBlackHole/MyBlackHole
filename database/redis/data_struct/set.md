# set

Redis Set是无序的，不可重复的String集合。 

与Set相关的常用命令： 
    SADD：向指定Set中添加1个或多个member，如果指定Set不存在，会自动创建一个。时间复杂度O(N)，N为添加的member个数 
    SREM：从指定Set中移除1个或多个member，时间复杂度O(N)，N为移除的member个数 
    SRANDMEMBER：从指定Set中随机返回1个或多个member，时间复杂度O(N)，N为返回的member个数 
    SPOP：从指定Set中随机移除并返回count个member，时间复杂度O(N)，N为移除的member个数 
    SCARD：返回指定Set中的member个数，时间复杂度O(1) 
    SISMEMBER：判断指定的value是否存在于指定Set中，时间复杂度O(1) 
    SMOVE：将指定member从一个Set移至另一个Set 
慎用的Set相关命令： 
    SMEMBERS：返回指定Hash中所有的member，时间复杂度O(N) 
    SUNION/SUNIONSTORE：计算多个Set的并集并返回/存储至另一个Set中，时间复杂度O(N)，N为参与计算的所有集合的总member数 
    SINTER/SINTERSTORE：计算多个Set的交集并返回/存储至另一个Set中，时间复杂度O(N)，N为参与计算的所有集合的总member数 
    SDIFF/SDIFFSTORE：计算1个Set与1或多个Set的差集并返回/存储至另一个Set中，时间复杂度O(N)，N为参与计算的所有集合的总member数 
    上述几个命令涉及的计算量大，应谨慎使用，特别是在参与计算的Set尺寸不可知的情况下，应严格避免使用。可以考虑通过SSCAN命令遍历获取相关Set的全部member（具体请见 https://redis.io/commands/scan ），如果需要做并集/交集/差集计算，可以在客户端进行，或在不服务实时查询请求的Slave上进行。 




- SMEMBERS
返回集合中所有成员

```shell
127.0.0.1:6379[1]> SMEMBERS _kombu.binding.192168110152
1) "192168110152\x06\x16\x06\x16192168110152"
```

## SADD

Redis Sadd 命令将一个或多个成员元素加入到集合中，已经存在于集合的成员元素将被忽略。
假如集合 key 不存在，则创建一个只包含添加的元素作成员的集合。
当集合 key 不是集合类型时，返回一个错误。

注意：在 Redis2.4 版本以前， SADD 只接受单个成员值。

- 语法
redis Sadd 命令基本语法如下：
redis 127.0.0.1:6379> SADD KEY_NAME VALUE1..VALUEN

```shell
redis 127.0.0.1:6379> SADD myset "hello"
(integer) 1
redis 127.0.0.1:6379> SADD myset "foo"
(integer) 1
redis 127.0.0.1:6379> SADD myset "hello"
(integer) 0
redis 127.0.0.1:6379> SMEMBERS myset
1) "hello"
2) "foo"
```
