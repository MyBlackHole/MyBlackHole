# signal

## SIGSEGV
SIG 是信号名的通用前缀， SEGV 是segmentation violation，也就是存储器区段错误。
1. 访问空指针
2. 内存越界访问
3. 访问已经释放的内存

- 如何避免SIGSEGV
申请内存之后，需要check 内存申请是否成功，然后再去访问内存。
确保申请的内存大小能满足使用的需求，避免越界访问。

![[imgs/signal.png]]
![[imgs/signal-1.png]]
## signal_struct

![[imgs/signal-2.png]]

## sighand_struct

![[imgs/signal-3.png]]
![[imgs/signal-4.png]]

## sigpending和sigqueue

![[imgs/signal-5.png]]
![[imgs/signal-6.png]]



## EPIPE
tcp 的三次握手中(是内核态完成的)，
也就是说，即使Server端一直不accept也不会影响Client端发送数据(接收缓冲区填满除外),
甚至在Client端调用close使Server端连接变成CLOSE_WAIT状态后依然可以成功accept并收取数据,
但这时如果发送数据的话会在Client端触发RST(指Client端的FIN_WAIT_2状态超时后连接已经销毁的情况), 
导致send操作返回EPIPE（errno 32）错误，并触发SIGPIPE信号（默认行为是Terminate）


客户端发送 SYN 包，服务端接收到 SYN 包，并发送 SYN+ACK 包，客户端接收到 SYN+ACK 包，并发送 ACK 包，此时服务端收到 ACK 包，并建立连接。
如果此时客户端发送数据，服务端由于没有接收到 ACK 包，会发送 RST 包，客户端收到 RST 包，会抛出 EPIPE 信号。

tcp 连接中，对端关闭了连接，发送端收到 EPIPE 信号，表示发送失败。

总结一下，应用程序会碰到EPIPE错误的场景有：
1. 在CLOSE_WAIT状态的连接上发送数据（对端已经关闭了连接），触发对端的RST；
2. 在本端socket上已经调用过shutdown(SEND_SHUTDOWN)；
