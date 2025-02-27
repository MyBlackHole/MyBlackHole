# checkpoint

当系统运行时间较长的时候，由于操作较多，日志文件的数量也较多。
如果每次利用日志进行恢复操作都会耗费大量的时间，为了节约时间同时减少不必要的恢复操作，引入了checkpoint的概念。
checkpoint表示在此操作之前，相关数据已经被保存到永久存储中，即使系统故障，这部分数据也不会丢失，因此恢复的时候只要从checkpoint操作之后根据日志执行恢复操作就可以了。
checkpoint本身也是一条xlog记录，该记录包含了redo点的位置，因此，每次恢复数据时，先从xloh记录里找到最近的一次checkpoint记录，并根据该记录找到相应的redo点位置，这就是执行本次恢复的起始点位置。
如图所示，checkpoint操作记录了redo点的位置。

![[imgs/checkpoint.png]]

