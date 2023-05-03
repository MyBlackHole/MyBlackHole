# kombu

redis:
    QOS:
处理过期(超时)的任务
maybe_restore_messages(self)
    client.zrevrangebyscore()
        restore_by_tag(tag, client)
            client.transaction
                _do_restore_message(M, EX, RK, pipe, leftmost)


启动两个 worder.py
设置 transport_options={"visibility_timeout": 10}
修改第一个 worder 在 unacked_index hash key 里的任务 id 的 member 小于 time() - visibility_timeout
例如:
127.0.0.1:6379> ZADD unacked_index  1683020451 "806c593d-1421-48fa-b570-eca6cf81b865"

启动第二个 worder 就会重复出现此现象
