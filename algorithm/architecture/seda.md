# seda

SEDA全称是：stage event driver architecture，中文直译为“分阶段的事件驱动架构”

![[imgs/Pasted image 20230827180453.png]]
1. 阶段控制器：StageController
2. 事件队列：eventQueue
3. 事件监听器
4. 事件处理线程池（线程池内部维护一个队列）
5. 事件处理器（事件处理器图上没显示，作为一个业务线程来实现）

处理流程如下：
1. event推送到stage的eventQueue
2.  EventListener监听到了event
3. ThreadPool异步并发处理event
# UML 
![[imgs/Pasted image 20230827180804.png]]
1. StageManager初始化调用boot()，创建Stage并维护Event和Stage之间的映射关系
2. Stage启动监听器，监听事件队列（事件队列可以是本地，也可以是分布式队列，这里采用一个Builder构造器了来实现，你可以在构造器里面实现自己的策略）
3. 如果接收到事件，将提交给执行器去执行（这里涉及到事件处理器，包含业务逻辑）
# PDM
1. t_stage_event：针对每个Stage的事件进行持久化，防止事件丢失补偿处理
2. t_stage_event_log: 对一个event做链路跟踪的日志表
![[imgs/Pasted image 20230827181156.png]]

> https://www.cnblogs.com/lay2017/p/10348413.html