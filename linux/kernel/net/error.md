# error

- connection reset by peer
```shell
服务器的并发连接数超过了其承载量，服务器会将其中一些连接关闭；
errno = 104错误表明你在对一个对端socket已经关闭的的连接调用write或send方法，在这种情况下，调用write或send方法后，对端socket便会向本端socket发送一个RESET信号，在此之后如果继续执行write或send操作，就会得到errno为104，错误描述为connection reset by peer。

具体的分析可以结合TCP的"四次挥手"关闭. TCP是全双工的信道, 可以看作两条单工信道, TCP连接两端的两个端点各负责一条. 当对端调用close时, 虽然本意是关闭整个两条信道, 但本端只是收到FIN包. 按照TCP协议的语义, 表示对端只是关闭了其所负责的那一条单工信道, 仍然可以继续接收数据. 也就是说, 因为TCP协议的限制, 一个端点无法获知对端的socket是调用了close还是shutdown.
对于一个TCP连接，如果对端执行close操作，则会向本端发送一个FIN分节，这时候读本端socket会返回0，我们就知道对方已经关闭了连接，通常这时候我们会在本地调用close来主动关闭本端连接。但如果对方socket已经执行了close的操作，本端socket还继续在这个连接上写数据，就会触发对端socket发送RST报文，按照TCP的四次挥手原理，这时候本端socket应该也要开始执行close的操作流程了，而不是接着发数据.
```

- 已经被废弃的 tcp_tw_recycle
[[sysctl]]
```shell

```
