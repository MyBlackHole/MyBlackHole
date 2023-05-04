# 数据结构

```dot
digraph kombu{

}
```

```plantuml
@startuml

class TokenBucket {
+ can_consume(self, tokens=1)
}

class FairCycle {
+ fun
+ resources
+ predicate
+ pos
}

abstract base.Transport {
+ Management
+ client
+ can_parse_url
+ default_port
+ connection_errors
+ channel_errors
+ driver_type
+ driver_name
+ __reader
+ implements
}

abstract virtual.Transport {
+ Channel
+ Cycle
+ Management
+ cycle
+ default_port
+ channels
+ _callbacks
+ polling_interval
+ channel_max
+ implements
__
+ drain_events(self, connection, timeout=None)
+ create_channel(self, connection)
}

class Redis.MltiChannelPoller {
+ eventflags
+ _in_protected_read
+ after_read
+ _channels
+ _fd_to_chan
+ _chan_to_sock
+ poller
}

class Redis.Transport {
+ Channel
+ polling_interval
+ default_port
+ driver_type
+ driver_name
+ implements
+ connection_errors
+ channel_errors
+ cycle
}

class virtual.Transport implements base.Transport
class Redis.Transport extends virtual.Transport

class Connection {
+ port
+ virtual_host
+ connect_timeout
+ _closed
+ _connection
+ _default_channel
+ _transport
+ _logger
+ uri_prefix
+ declared_entities
+ cycle
+ transport_options
+ failover_strategy
+ heartbeat
+ resolve_aliases
+ failover_strategies
+ hostname
+ userid
+ password
+ ssl
+ login_method
__
+ drain_events()
+ transport()
+ clone(self, **kwargs)
}

class ConsumerMixin {
+ connect_max_retries
+ should_stop
__
+ run(self, _tokens=1, **kwargs)
+ __init__()
+ consume(self, limit=None, timeout=None, safety_interval=1, **kwargs)
+ consumer_context(self, **kwargs)
+ extra_context(self, connection, channel)
+ Consumer(self)
}

class Worker {
+ connection
__
+ __init__(self, connection)
+ process_task(self, body, message)
+ get_consumers(self, Consumer, channel)
}

class Worker extends ConsumerMixin
Worker::connection --> Connection
Connection::_transport --> Redis.Transport
Redis.Transport::cycle --> Redis.MltiChannelPoller

@enduml
```

```plantuml
|main|
start
:Connection() as conn;
|worker|
:Worker(conn).run();
|ConsumerMixin|
:ConsumerMixin.run();
|TokenBucket|
:TokenBucket(1).can_consume(1);
:self._get_tokens();
|ConsumerMixin|
:consume();
:consumer_context();
:Consumer();
:establish_connection();
|Connection|
:conn.default_channel();
:conn.drain_events(timeout=safety_interval);
:transport(self);
:create_transport(self);
:get_transport_cls(self);
|transport|
:get_transport_cls(transport_cls);
:resolve_transport(transport=None);
|redis.transport|
:transport.drain_events(self.connection, **kwargs);
|base.transport|
:transport.drain_events(self.connection, **kwargs);
|MultiChannelPoller|
:cycle.get();
|redis.transport|
:get(self, callback, timeout=None);
|eventio|
:self.poller.poll(timeout);
```
