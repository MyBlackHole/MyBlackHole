# key

|   |   |
|---|---|
|DEL key [key …]|该命令用于在 key 存在时删除 key|
|D MP key|序列化给定 key ，并返回被序列化的值。|
|EXISTS key|检查给定 key  是否存在。|
|EXPIRE key seconds|为给定 key 设置过期时间。|
|EXPIRE T key timest mp|EXPIRE T 的作用和 EXPIRE 类似，都用于为 key 设置过期时间。 不同在于 EXPIRE T 命令接受的时间参数是  NIX 时间戳( nix timest mp)。|
|PEXPIRE key milliseconds|设置 key 的过期时间亿以毫秒计。|
|PEXPIRE T key milliseconds-timest mp|设置 key 过期时间的时间戳( nix timest mp) 以毫秒计|
|KEYS p ttern|查找所有符合给定模式( p ttern)的 key|
|keys str*|列出所有str开头的key|
|SC N c rsor [M TCH p ttern] [CO NT co nt]|迭代数据库中的键 |
|MOVE key db|将当前数据库的 key 移动到给定的数据库 db 当中。|
|PERSIST key|移除 key 的过期时间，key 将持久保持。|
|PTTL key|以毫秒为单位返回 key 的剩余的过期时间。|
|TTL key|以秒为单位，返回给定 key 的剩余生存时间(TTL, time to live)。过期返回 -2， 永久返回 -1.|
|R NDOMKEY|从当前数据库中随机返回一个 key 。|
|REN ME key newkey|修改 key 的名称|
|REN MENX key newkey|仅当 newkey 不存在时，将 key 改名为 newkey 。|
|TYPE key|返回 key 所储存的值的类型。|

## 发布订阅

|   |   |
|---|---|
|PSUBSCRIBE pattern [pattern ...]|订阅一个或多个符合给定模式的频道。|
|PUBSUB subcommand [argument [argument ...]]|查看订阅与发布系统状态。|
|PUBLISH channel message|将信息发送到指定的频道。|
|PUNSUBSCRIBE [pattern [pattern ...]]|退订所有给定模式的频道。|
|SUBSCRIBE channel [channel ...]|订阅给定的一个或多个频道的信息。|
|UNSUBSCRIBE [channel [channel ...]]|指退订给定的频道。|

## 事务

|   |   |
|---|---|
|DISCARD|取消事务，放弃执行事务块内的所有命令。|
|EXEC|执行所有事务块内的命令。|
|MULTI|标记一个事务块的开始。|
|UNWATCH|取消 WATCH 命令对所有 key 的监视。|
|WATCH key [key ...]|监视一个(或多个) key ，如果在事务执行之前这个(或这些) key 被其他命令所改动，那么事务将被打断。|

## 服务器

|   |   |
|---|---|
|BGREWRITEAOF|异步执行一个 AOF（AppendOnly File） 文件重写操作|
|BGSAVE|在后台异步保存当前数据库的数据到磁盘|
|CLIENT KILL [ip:port] [ID client-id]|关闭客户端连接|
|CLIENT LIST|获取连接到服务器的客户端连接列表|
|CLIENT GETNAME|获取连接的名称|
|CLIENT PAUSE timeout|在指定时间内终止运行来自客户端的命令|
|CLIENT SETNAME connection-name|设置当前连接的名称|
|CLUSTER SLOTS|获取集群节点的映射数组|
|COMMAND|获取 Redis 命令详情数组|
|COMMAND COUNT|获取 Redis 命令总数|
|COMMAND GETKEYS|获取给定命令的所有键|
|TIME|返回当前服务器时间|
|COMMAND INFO command-name [command-name ...]|获取指定 Redis 命令描述的数组|
|CONFIG GET parameter|获取指定配置参数的值|
|CONFIG REWRITE|对启动 Redis 服务器时所指定的 redis.conf 配置文件进行改写|
|CONFIG SET parameter value|修改 redis 配置参数，无需重启|
|CONFIG RESETSTAT|重置 INFO 命令中的某些统计数据|
|DBSIZE|返回当前数据库的 key 的数量|
|DEBUG OBJECT key|获取 key 的调试信息|
|DEBUG SEGFAULT|让 Redis 服务崩溃|
|FLUSHALL|删除所有数据库的所有key|
|FLUSHDB|删除当前数据库的所有key|
|INFO [section]|获取 Redis 服务器的各种信息和统计数值|
|LASTSAVE|返回最近一次 Redis 成功将数据保存到磁盘上的时间，以 UNIX 时间戳格式表示|
|MONITOR|实时打印出 Redis 服务器接收到的命令，调试用|
|ROLE|返回主从实例所属的角色|
|SAVE|异步保存数据到硬盘|
|SHUTDOWN [NOSAVE] [SAVE]|异步保存数据到硬盘，并关闭服务器|
|SLAVEOF host port|将当前服务器转变为指定服务器的从属服务器(slave server)|
|SLOWLOG subcommand [argument]|管理 redis 的慢日志|
|SYNC|用于复制功能(replication)的内部命令|

## lua

```lua
eval"return {KEYS[1],KEYS[2],ARGV[1],ARGV[2]}"2key1key2firstsecond
```

```lua
Del-batch.lua: 
-- 定义游标cur初始值为0 
local cur = 0 
-- 定义删除个数初始值 
local count=0 
-- 循环调用 
repeat 
    -- 调用游标 
    local result = redis.call("scan",cur,"match",KEYS[1]) 
    -- 将下个游标点转化为number 
    cur = tonumber(result[1]) 
    local arr = result[2] 
    -- 循环当前游标获取到的值，进行删除 
    if(arr~=nil and #arr>0) then 
        for i,k in pairs(arr) do 
            local key = tostring(k) 
            -- 或者使用redis.call("unlink",key) 
            redis.call("del",key) 
            count = count +1 
        end 
    end 
-- 当游标点为0时，退出循环 
until(cur<=0) 
-- 返回执行的结果 
return "del pattern is : "..KEYS[1]..", count is:"..count
redis-cli --eval del-batch.lua "TEST_KEY*"
```
