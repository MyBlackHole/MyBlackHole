# string

SET key value [EX…]

|   |   |
|---|---|
|EX seconds|将键的过期时间设置为 seconds 秒。 执行 SET key value EX seconds 的效果等同于执行 SETEX key seconds value 。|
|PX milliseconds|将键的过期时间设置为 milliseconds 毫秒。 执行 SET key value PX milliseconds 的效果等同于执行 PSETEX key milliseconds value 。|
|NX|只在键不存在时， 才对键进行设置操作。 执行 SET key value NX 的效果等同于执行 SETNX key value 。|
|XX|只在键已经存在时， 才对键进行设置操作。|

|   |   |
|---|---|
|GET key|获取指定 key 的值。|
|GETRANGE key start end|返回 key 中字符串值的子字符|
|GETSET key value|将给定 key 的值设为 value ，并返回 key 的旧值(old value)。|
|GETBIT key offset|对 key 所储存的字符串值，获取指定偏移量上的位(bit)。|
|MGET key1 [key2..]|获取所有(一个或多个)给定 key 的值。|
|SETBIT key offset value|对 key 所储存的字符串值，设置或清除指定偏移量上的位(bit)。|
|SETEX key seconds value|将值 value 关联到 key ，并将 key 的过期时间设为 seconds (以秒为单位)。|
|SETNX key value|只有在 key 不存在时设置 key 的值。|
|SETRANGE key offset value|用 value 参数覆写给定 key 所储存的字符串值，从偏移量 offset 开始。|
|STRLEN key|返回 key 所储存的字符串值的长度。|
|MSET key value [key value ...]|同时设置一个或多个 key-value 对。|
|MSETNX key value [key value ...]|同时设置一个或多个 key-value 对，当且仅当所有给定 key 都不存在。|
|PSETEX key milliseconds value|这个命令和 SETEX 命令相似，但它以毫秒为单位设置 key 的生存时间，而不是像 SETEX 命令那样，以秒为单位。|
|INCR key|将 key 中储存的数字值增一。|
|INCRBY key increment|将 key 所储存的值加上给定的增量值（increment） 。|
|INCRBYFLOAT key increment|将 key 所储存的值加上给定的浮点增量值（increment） 。|
|DECR key|将 key 中储存的数字值减一。|
|DECRBY key decrement|key 所储存的值减去给定的减量值（decrement） 。|
|APPEND key value|如果 key 已经存在并且是一个字符串， APPEND 命令将 value 追加到 key 原来的值的末尾。|
|setbit key offset value|二进制 给键 key 的 offset 位 设置成 value|
|getbit key offset|二进制 获取 key 的 offset 位的值|
|bitop and/or/xor/not destkey [key…]|对多个 key 求逻辑并/逻辑或/逻辑异或/逻辑非，并将结果保存到 destkey中|