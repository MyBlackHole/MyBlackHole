# AMQP

[细节](https://www.cnblogs.com/itbsl/p/14421140.html)
[细节](https://blog.csdn.net/zhoupenghui168/article/details/130322210)
Advanced Message Queuing Protocol(高级消息协议)
通用软件总线
一个提供统一消息服务的应用层标准高级消息队列协议，是应用层协议的一个开放标准,为面向消息的中间件设计。基于此协议的客户端与消息中间件可传递消息，并不受客户端/中间件同产品，不同的开发语言等条件的限制

## 层级
AMQP协议这种降低耦合的机制是基于与上层产品，语言无关的协议
是一种二进制协议，提供客户端应用与消息中间件之间多通道、协商、异步、安全、中立和高效地交互
从整体来看，AMQP协议可划分为两层

- Functional layer
    功能层，位于协议上层主要定义了一组命令（基于功能的逻辑分类），用于应用程序调用实现自身所需的业务逻辑。例如：应用程序可以通过功能层定义队列名称，生产消息到指定队列，消费指定队列消息等基于（Message queues 模型）

- transport layer
    传输层，基于二进制数据流传输，用于将应用程序调用的指令传回服务器，并返回结果，同时可以处理信道复用，帧处理，内容编码，心跳传输，数据传输和异常处理。

## 设计要求
### 模型设计驱动

1. 保证基于模型实现的应用之间相互可以联通；
2. 提供对服务质量的可靠控制；
3. 命名规划，要求命名明确且保持一致；
4. 允许通过协议配置服务器连接；
5. 功能层命名能够简单的映射到应用程序级别的 API；
6. 职责单一明确，每个操作只做一件事情。

### 传输层设计驱动
1. 使用二进制数据流压缩和解压，提高效率；
2. 可以处理任意大小的消息，且不做任何限制；
3. 单个连接支持多个通信通道；
4. 客户端和服务端基于长链接实现，且无特殊限制；
5. 允许异步指令基于管道通信；
6. 易扩展，基于新的需求和变化支持扩展；
7. 新版本向下兼容老版本；
8. 基于断言模型，异常可以快速定位修复；
9. 对编程语言保持中立；
10. 适应代码发展演变；

## 模型组件
### Message Queue
消息队列会将消息存储到内存或者磁盘中，并将这些消息按照一定顺序转发给一个或者多个消费者，每个消息队列都是独立隔离的，相互不影响。

消息队列具有不同的属性：私有，共享，持久化，临时，客户端定义 或者服务端定义等，可以基于实际需求选择对应的类型，以 RabbitMQ 队列特性为例：

共享持久化消息队列：将发送的消息存储到磁盘，然后将消息转发给订阅该队列的所有消费者；

私有临时消息队列：RabbitMQ 支持 rpc 调用，再调用过程中消费者都会临时生成一个消息队列，只有当前消费者可见，且由服务端生成，调用完就会销毁队列。

### Exchange
交换机收到生产者投递的消息，基于路由规则及队列绑定关系匹配到投递对应的交换机或者队列进行分发，交换机不存储消息，只做转发。

AMQP定义了许多标准交换类型，基本涵盖了消息传递所需的路由类型，一般 AMQP 服务器都会提供默认的交换机基于交换机类型命名，AMQP 的应用程序也可以创建自己的交换机用于绑定指定的消息队列发布消息。
![[imgs/AMQP.png]]
- 消息生命周期
消息主要由属性及消息内容组成，生产者创建消息时可以给消息设置属性及消息内容，同时也可以标记路由信息在消息上，可以将消息发送到指定交换机。

当消息到达交换机时，交换机会基于路由规则判断消息能否转发，如果不能转发会丢弃消息同时反馈给生产者。

交换机基于路由规则可以将消息投递到一个或者多个消息队列，服务器通过复制或者计数器的方式将消息保存到不同队列中，每个队列中的消息内容是相同的，但是操作是隔离的，相互不影响。

当消息到达消息队列后，消息队列会基于 AMQP 协议投递给消费者，如果无法投递给消费者或者没有消费者，消息将在内存或者磁盘中存储，等待消费者。

当消息队列可以将消息传递给消费者时，消息将从其内部缓冲区中删除。 删除操作可能立刻执行也可以再消费者确认消息消费后再执行，删除策略消费者可以选择。

生产消息投递确认和消费消息消费确认可以作为两个事务，然后提交或者回滚事务。

## 指令架构
### 协议指令
作为消息中间件传统的 API 定义的操作非常复杂，为了解决这个问题 AMQP 基于传统 API 的功能，定义方法来对应实现 API 的操作每个方法只完成一件事，通过方法之间的组合来实现完整的功能，所以AMQP 形成了一个非常庞大的指令集，但是指令集中的方法都是便于理解的。

AMQP 指令集中指令，基于对应的特定功能域被划分为不同的类，其中有一些类作为特定类的支持类，属于可选的。
例如:
- 同步请求
一边等待对方发送请求，一边等待对方发送回复。适用于对性能要求不高的场景。
- 异步请求
发送请求后不等待回复，使用场景对性能要求比较高的场景。

为了简化指令处理，我们给每个同步请求定义不同的回复指令，也就是说同一个回复指令不可能返回给2个不同的请求
这也意味着发送同步请求的发送方可以接受和处理回复的指令，直到获得有效的同步回复指令为止
这种方式可以将 AMQP 与传统的 RPC 协议区分开来。

一条指令可以被定义为同步请求，同步回复（针对特定请求）或者异步回复，但是每种指令真正再被定义是在客户端（即服务器到客户端）或者服务端（即户端到服务器）。
### AMQP 映射到中间层 API
AMQP 映射到中间层 API，这个映射过程并不是所有方法和参数完全映射，因为有部分方法或者参数对应用程序没有意义。同时映射规则也是固定的，基于已定的一些规则，所有方法按照这个规则映射，不需要人工干预。
例如：队列声明
```
Queue.Declare 
    queue=my.queue
    auto-delete=TRUE 
    exclusive=FALSE
```
可以作为一条线性记录
```
+--------+---------+----------+-----------+-----------+ 
|  Queue | Declare | my.queue |     1     |     0     | 
+--------+---------+----------+-----------+-----------+ 
   class    method     name    auto-delete  exclusive
```
也可以作为高级 API
```
queue_declare (session, "my.queue", TRUE, FALSE);
```
对于大多数应用程序来说，中间层（指令层）隐藏再技术层面，应用程序实际使用的 API 功能对比中间层相对会较少。

## 传输层架构
### 概述
AMQP 传输基于二进制协议，传输的信息被组织成各种类型的帧，帧携带协议方法和其他相关信息，所有的帧具有相同个格式：帧头，有效内容，帧尾。帧的有效内容格式取决于帧的类型。

假设再一个可靠的面向流的网络传输层（例如：TCP / IP）

再一个 Socket 连接中，可以有多个独立的线程访问，这种情况就是上文中提到的 Channel（通道），每个帧都有一个属于自己的通道号码，再同一个连接中所有的帧混合在一起，不同的通道共享连接，但是针对每个通道自身的帧都是按照严格的顺序运行。

由于帧的有效内容都是由帧头和帧尾包装，所以对应帧数据的解析是相当简单便捷的，同时基于协议规范生成帧数据也是非常容易。

### 数据类型
AMQP 使用的数据类型如下：

- Integers（数值范围1-8， 8个字节）：用于表示大小，数量，限制等，整数类型无符号的，可以在帧内不对齐。
- Bits（统一为8个字节）：用于表示开/关值。
- Short strings：用于保存简短的文本属性，字符串个数限制为255，8个字节
- Long strings：用于保存二进制数据块。
- Field tables：包含键值对，字段值一般为字符串，整数等。

### 协议协商
AMQP 客户端和服务器存在协商协议。这意味着当客户端连接时，服务端会提出一些客户端可以接受或者修改的选项，如果双方达成一致，连接继续，基于协商协议，可以设定好一些先决条件。

在AMQP中，协商协议的一些具体方面：

- 实际的协议和版本。服务器可以在同一端口上监听多个协议。
- 加密参数和双方的身份验证。
- 最大帧大小，通道数量和其他操作限制。
