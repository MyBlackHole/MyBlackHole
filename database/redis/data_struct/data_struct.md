# 数据结构
- key
Redis采用Key-Value型的基本数据结构，任何二进制序列都可以作为Redis的Key使用（例如普通的字符串或一张JPEG图片） 
关于Key的一些注意事项： 
    不要使用过长的Key。例如使用一个1024字节的key就不是一个好主意，不仅会消耗更多的内存，还会导致查找的效率降低 
    Key短到缺失了可读性也是不好的，例如"u1000flw"比起"user:1000:followers"来说，节省了寥寥的存储空间，却引发了可读性和可维护性上的麻烦 
    最好使用统一的规范来设计Key，比如"object-type:id:attr"，以这一规范设计出的Key可能是"user:1000"或"comment:1234:reply-to" 
    Redis允许的最大Key长度是512MB（对Value的长度限制也是512MB） 

- string
String是Redis的基础数据类型，Redis没有Int、Float、Boolean等数据类型的概念，所有的基本类型在Redis中都以String体现。 

与String相关的常用命令： 
    SET：为一个key设置value，可以配合EX/PX参数指定key的有效期，通过NX/XX参数针对key是否存在的情况进行区别操作，时间复杂度O(1) 
    GET：获取某个key对应的value，时间复杂度O(1) 
    GETSET：为一个key设置value，并返回该key的原value，时间复杂度O(1) 
    MSET：为多个key设置value，时间复杂度O(N) 
    MSETNX：同MSET，如果指定的key中有任意一个已存在，则不进行任何操作，时间复杂度O(N) 
    MGET：获取多个key对应的value，时间复杂度O(N) 
    上文提到过，Redis的基本数据类型只有String，但Redis可以把String作为整型或浮点型数字来使用，主要体现在INCR、DECR类的命令上： 
    INCR：将key对应的value值自增1，并返回自增后的值。只对可以转换为整型的String数据起作用。时间复杂度O(1) 
    INCRBY：将key对应的value值自增指定的整型数值，并返回自增后的值。只对可以转换为整型的String数据起作用。时间复杂度O(1) 
    DECR/DECRBY：同INCR/INCRBY，自增改为自减。 
    INCR/DECR系列命令要求操作的value类型为String，并可以转换为64位带符号的整型数字，否则会返回错误。 
    也就是说，进行INCR/DECR系列命令的value，必须在[-2^63 ~ 2^63 - 1]范围内。 
    前文提到过，Redis采用单线程模型，天然是线程安全的，这使得INCR/DECR命令可以非常便利的实现高并发场景下的精确控制。 

- hash
Hash即哈希表，Redis的Hash和传统的哈希表一样，是一种field-value型的数据结构，可以理解成将HashMap搬入Redis。 
Hash非常适合用于表现对象类型的数据，用Hash中的field对应对象的field即可。 
Hash的优点包括： 
可以实现二元查找，如"查找ID为1000的用户的年龄" 
比起将整个对象序列化后作为String存储的方法，Hash能够有效地减少网络传输的消耗 
当使用Hash维护一个集合时，提供了比List效率高得多的随机访问命令 

与Hash相关的常用命令： 
    HSET：将key对应的Hash中的field设置为value。如果该Hash不存在，会自动创建一个。时间复杂度O(1) 
    HGET：返回指定Hash中field字段的值，时间复杂度O(1) 
    HMSET/HMGET：同HSET和HGET，可以批量操作同一个key下的多个field，时间复杂度：O(N)，N为一次操作的field数量 
    HSETNX：同HSET，但如field已经存在，HSETNX不会进行任何操作，时间复杂度O(1) 
    HEXISTS：判断指定Hash中field是否存在，存在返回1，不存在返回0，时间复杂度O(1) 
    HDEL：删除指定Hash中的field（1个或多个），时间复杂度：O(N)，N为操作的field数量 
    HINCRBY：同INCRBY命令，对指定Hash中的一个field进行INCRBY，时间复杂度O(1) 
应谨慎使用的Hash相关命令： 
    HGETALL：返回指定Hash中所有的field-value对。返回结果为数组，数组中field和value交替出现。时间复杂度O(N) 
    HKEYS/HVALS：返回指定Hash中所有的field/value，时间复杂度O(N) 
    上述三个命令都会对Hash进行完整遍历，Hash中的field数量与命令的耗时线性相关，对于尺寸不可预知的Hash，应严格避免使用上面三个命令，而改为使用HSCAN命令进行游标式的遍历，具体请见 https://redis.io/commands/scan 

- sorted set
Redis Sorted Set是有序的、不可重复的String集合。Sorted Set中的每个元素都需要指派一个分数(score)，Sorted Set会根据score对元素进行升序排序。如果多个member拥有相同的score，则以字典序进行升序排序
Sorted Set非常适合用于实现排名。 
Sorted Set的主要命令： 
    ZADD：向指定Sorted Set中添加1个或多个member，时间复杂度O(Mlog(N))，M为添加的member数量，N为Sorted Set中的member数量 
    ZREM：从指定Sorted Set中删除1个或多个member，时间复杂度O(Mlog(N))，M为删除的member数量，N为Sorted Set中的member数量 
    ZCOUNT：返回指定Sorted Set中指定score范围内的member数量，时间复杂度：O(log(N)) 
    ZCARD：返回指定Sorted Set中的member数量，时间复杂度O(1) 
    ZSCORE：返回指定Sorted Set中指定member的score，时间复杂度O(1) 
    ZRANK/ZREVRANK：返回指定member在Sorted Set中的排名，ZRANK返回按升序排序的排名，ZREVRANK则返回按降序排序的排名。时间复杂度O(log(N)) 
    ZINCRBY：同INCRBY，对指定Sorted Set中的指定member的score进行自增，时间复杂度O(log(N)) 
慎用的Sorted Set相关命令： 
    ZRANGE/ZREVRANGE：返回指定Sorted Set中指定排名范围内的所有member，ZRANGE为按score升序排序，ZREVRANGE为按score降序排序，时间复杂度O(log(N)+M)，M为本次返回的member数 
    ZRANGEBYSCORE/ZREVRANGEBYSCORE：返回指定Sorted Set中指定score范围内的所有member，返回结果以升序/降序排序，min和max可以指定为-inf和+inf，代表返回所有的member。时间复杂度O(log(N)+M) 
    ZREMRANGEBYRANK/ZREMRANGEBYSCORE：移除Sorted Set中指定排名范围/指定score范围内的所有member。时间复杂度O(log(N)+M) 
    上述几个命令，应尽量避免传递[0 -1]或[-inf +inf]这样的参数，来对Sorted Set做一次性的完整遍历，特别是在Sorted Set的尺寸不可预知的情况下。可以通过ZSCAN命令来进行游标式的遍历（具体请见 https://redis.io/commands/scan ），或通过LIMIT参数来限制返回member的数量（适用于ZRANGEBYSCORE和ZREVRANGEBYSCORE命令），以实现游标式的遍历。 

- bitmap\hyperloglog
Redis的这两种数据结构相较之前的并不常用，在本文中只做简要介绍，如想要详细了解这两种数据结构与其相关的命令，请参考官方文档https://redis.io/topics/data-types-intro 中的相关章节 
Bitmap在Redis中不是一种实际的数据类型，而是一种将String作为Bitmap使用的方法。可以理解为将String转换为bit数组。使用Bitmap来存储true/false类型的简单数据极为节省空间。 
HyperLogLogs是一种主要用于数量统计的数据结构，它和Set类似，维护一个不可重复的String集合，但是HyperLogLogs并不维护具体的member内容，只维护member的个数。也就是说，HyperLogLogs只能用于计算一个集合中不重复的元素数量，所以它比Set要节省很多内存空间。 

- other
    EXISTS：判断指定的key是否存在，返回1代表存在，0代表不存在，时间复杂度O(1) 
    DEL：删除指定的key及其对应的value，时间复杂度O(N)，N为删除的key数量 
    EXPIRE/PEXPIRE：为一个key设置有效期，单位为秒或毫秒，时间复杂度O(1) 
    TTL/PTTL：返回一个key剩余的有效时间，单位为秒或毫秒，时间复杂度O(1) 
    RENAME/RENAMENX：将key重命名为newkey。使用RENAME时，如果newkey已经存在，其值会被覆盖；使用RENAMENX时，如果newkey已经存在，则不会进行任何操作，时间复杂度O(1) 
    TYPE：返回指定key的类型，string, list, set, zset, hash。时间复杂度O(1) 
    CONFIG GET：获得Redis某配置项的当前值，可以使用*通配符，时间复杂度O(1) 
    CONFIG SET：为Redis某个配置项设置新值，时间复杂度O(1) 
    CONFIG REWRITE：让Redis重新加载redis.conf中的配置 
