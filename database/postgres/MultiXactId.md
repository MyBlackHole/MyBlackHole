# MultiXactId

PG 中用tuple上的 xmin,xmax 记录产生这一行的txid,与删除这一行的txid,并且用来实现锁机制,
exclusive模式的锁只被一个事务持有，可以通过xmax中获取持有该锁的txid.很好理解. shared 模式的锁,
是可以被多个事务共同持有的,怎么记录被哪些事务持有呢？
如何使用一个txid标识一组事务？Multixact就是做这个事的，用multixactid标识出一组事务。
