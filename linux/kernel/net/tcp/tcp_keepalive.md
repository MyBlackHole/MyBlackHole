# tcp_keepalive

```shell
int keepAlive = 1;    // 非0值，开启keepalive属性
int keepIdle = 60;    // 如该连接在60秒内没有任何数据往来,则进行此TCP层的探测
int keepInterval = 5; // 探测发包间隔为5秒
int keepCount = 3;        // 尝试探测的最多次数

// 开启探活
setsockopt(sockfd, SOL_SOCKET, SO_KEEPALIVE, (void *)&keepAlive, sizeof(keepAlive));
setsockopt(sockfd, SOL_TCP, TCP_KEEPIDLE, (void*)&keepIdle, sizeof(keepIdle));
setsockopt(sockfd, SOL_TCP, TCP_KEEPINTVL, (void *)&keepInterval, sizeof(keepInterval));
setsockopt(sockfd, SOL_TCP, TCP_KEEPCNT, (void *)&keepCount, sizeof(keepCount) 
```
