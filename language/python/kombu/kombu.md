# kombu

## 概念
- Producers: 发送消息给exchange
- Exchanges: 用于路由消息（消息发给exchange，exchange发给对应的queue）。路由就是比较routing-key（这个message提供）和binding-key（这个queue注册到exchange的时候提供）。使用时，需要指定exchange的名称和类型（direct，topic和fanout）。可以发现，和RabbitMQ中的exchange概念是一样的。
- Consumers: consumer需要声明一个queue，并将queue与指定的exchange绑定，然后从queue里面接收消息。
- Queues: 接收exchange发过来的消息
- Routing keys: 每个消息在发送时都会声明一个routing_key。routing_key的含义依赖于exchange的类型。一般说来，在AMQP标准里定义了四种默认的exchange类型，此外，vendor还可以自定义exchange的类型。但是，我们下面只关注AMQP 0.8版本中定义的三种默认exchange类型，也是最常用的三类exchange。
    - Direct exchange: 如果message的routing_key和某个consumer中的routing_key相同，就会把消息发送给这个consumer监听的queue中。
    - Fan-out exchange: 广播模式。exchange将收到的message发送到所有与之绑定的queue中。
    - Topic exchange: 该类型exchange会将message发送到与之routing_key类型相匹配的queue中。routing_key由一系列“.”隔开的word组成，“*”代表匹配任何word，“#”代表匹配0个或多个word，类似于正则表达式。


## 处理过期
redis:
    QOS:
处理过期(超时)的任务


kombu/transport/redis.py:Transport.register_with_event_loop
    kombu/transport/redis.py:MultiChannelPoller.maybe_restore_messages
        kombu/transport/redis.py:QoS.restore_visible
            kombu/transport/redis.py:Channel.conn_or_acquire as clien
                redis.StrictRedis.zrevrangebyscore(unacked_index)
                kombu/transport/redis.py:QoS.restore_by_tag
                    kombu/transport/redis.py:QoS.restore_by_tag.restore_transaction
                        kombu/transport/redis.py:QoS.restore_by_tag._remove_from_indices
                        kombu/transport/redis.py:Channel._do_restore_message
                            kombu/transport/virtual/base.py:Channel._lookup
                                kombu/transport/virtual/exchange.py:DirectExchange(根据不同 exchange).lookup
                            redis.StrictRedis.lpush


启动两个 worder.py
设置 transport_options={"visibility_timeout": 10}
修改第一个 worder 在 unacked_index hash key 里的任务 id 的 member 小于 time() - visibility_timeout
例如:
127.0.0.1:6379> ZADD unacked_index  1683020451 "806c593d-1421-48fa-b570-eca6cf81b865"

启动第二个 worder 就会重复出现此现象
