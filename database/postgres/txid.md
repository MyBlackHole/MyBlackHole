# txid

txid 通过比较大小来判断是否可见，任何事务只可见 txid < 其自身 txid 的事务修改结果

**txid是无符号的32位整型，它并不是无限的** txid 数量有限
pg 将 txid 空间视为一个环，若不进行特殊处理，txid到达最大值后又会从3开始分配（0-2保留），如果进行简单的比大小，之前的事务就可以看到这个新事务创建的元组，而新事务不能看到之前事务创建的元组，这违反了事务的可见性。这种现象称为PG的事务ID回卷问题

## 事务ID的比较方法

实际上虽然txid空间有42亿，却并非按实际数字大小来判断可见性。
pg将txid空间一分为二，对于某个特定的txid，其后约21亿个txid属于未来，均不可见；
其前约21亿个txid属于过去，均可见。

例如对于txid=100的事务，从101到2^31+100均为不可见事务(即n+1到n+2^31)；
从2^31+101到99均为可见事务（即n+2^31+1到n-1)

![[imgs/txid.png]]

```c
/*
 * TransactionIdPrecedes --- is id1 logically < id2?
 */
bool TransactionIdPrecedes(TransactionId id1, TransactionId id2) // 结果返回一个bool值
{
	/*
	 * If either ID is a permanent XID then we can just do unsigned
	 * comparison.  If both are normal, do a modulo-2^32 comparison.
	 */
	int32		diff;
 
	if (!TransactionIdIsNormal(id1) || !TransactionIdIsNormal(id2)) //若其中一个不是普通id，则其一定较新（较大）
		return (id1 < id2);
 
	diff = (int32) (id1 - id2);
	return (diff < 0);
}
```
