# error

- 无法从连接池获取到连接
1. 客户端：高并发下连接池设置过小，出现供不应求，所以会出现上面的错误，但是正常情况下只要比默认的最大连接数(8个)多一些即可，因为正常情况下JedisPool以及Jedis的处理效率足够高。  
2. 客户端：没有正确使用连接池，比如没有进行释放，例如下面代码所示： 定义JedisPool，使用默认的连接池配置。 
3. 客户端：存在慢查询操作，这些慢查询持有的Jedis对象归还速度会比较慢，造成池子满了。  
4. 服务端：客户端是正常的，但是Redis服务端由于一些原因造成了客户端命令执行过程的阻塞，也会使得客户端抛出这种异常。 

- 客户端读写超时
1. 读写超时设置的过短。 
2. 命令本身就比较慢。 
3. 客户端与服务端网络不正常。 
4. Redis自身发生阻塞。 

- 客户端连接超时
1. 连接超时设置的过短。 
2. Redis发生阻塞，造成tcp-backlog已满，造成新的连接失败。 
3. 客户端与服务端网络不正常。 

- 客户端缓冲区异常
1. 输出缓冲区满。例如将普通客户端的输出缓冲区设置为1M 1M 60： 
2. 长时间闲置连接被服务端主动断开，可以查询timeout配置的设置以及自身连接池配置是否需要做空闲检测。  
3. 不正常并发读写：Jedis对象同时被多个线程并发操作，可能会出现上述异常。 
