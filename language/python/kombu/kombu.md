# kombu

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
