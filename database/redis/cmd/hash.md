# hash

|   |   |
|---|---|
|HDEL key field2 [field2]|删除一个或多个哈希表字段|
|HEXISTS key field|查看哈希表 key 中，指定的字段是否存在。|
|HGET key field|获取存储在哈希表中指定字段的值|
|HGETALL key|获取在哈希表中指定 key 的所有字段和值|
|HINCRBY key field increment|为哈希表 key 中的指定字段的整数值加上增量 increment 。|
|HINCRBYFLOAT key field increment|为哈希表 key 中的指定字段的浮点数值加上增量 increment 。|
|HKEYS key|获取所有哈希表中的字段|
|HLEN key|获取哈希表中字段的数量|
|HMGET key field1 [field2]|获取所有给定字段的值|
|HMSET key field1 value1 [field2 value2 ]|同时将多个 field-value (域-值)对设置到哈希表 key 中。|
|HSET key field value|将哈希表 key 中的字段 field 的值设为 value 。|
|HSETNX key field value|只有在字段 field 不存在时，设置哈希表字段的值。|
|HVALS key|迭代哈希表中的键值对。获取哈希表中所有值|
|HSCAN key cursor [MATCH pattern] [COUNT count]|迭代哈希表中的键值对。|