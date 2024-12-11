# preempt

```c
// 关闭抢占
preempt_disable()

临界区

// 恢复抢占
preempt_enable()
```

## 为什么上锁前要关闭抢占？

### 单核CPU

task2 优先级高于 task1
或者
发生中断需要运行 task2

task 1 需要访问共享资源前先获取 lock 后进入共享资源临界区, 此时发生抢占,
调度到 task 2, task 2 也需要访问共享资源, 但此时 lock 已被 task 1 持有,
task 2 一直无法获取 lock，也无法调度到 task 1, 造成死锁。

### 多核CPU




## spin_lock 为什么要关闭抢占？

Priority inversion:

是指“优先级反转”，也就是优先级高的任务反而较晚完成的错误现象；比如，假设有进程A，B，C；
优先级A>B>C;A，C进程都需要临界区数据；而B不需要；
    如果某个条件下，A进程得到机会执行，但C却持有它需要的锁，所以A虽然得到CPU但是却只能等待；
CPU上又回放回C；接着B试图执行，它可以将C挤出去，因为它的优先级高于C，而且它也不需要C持有的锁；
B执行结束后，C又回到CPU上继续执行，直到它结束并且放开锁，A才有可能得到锁以便完成任务。那么
最终结果就是，B最先完成，接着是C，最后是A。这个结果就与他们原来定义的优先级不同了，优先级最高
的任务反而最后完成。
    如果这里禁止抢占，那么调度就不需要考虑优先级，仅仅依赖于是否可以获得锁。当C持有锁的时候，
A会等待；而B不会被调度进来，因为优先级判断已经失效了。只能等C执行结束，才有机会调度A或者B。
这看起来就是spin_lock想要的效果。但是这种情况里，就没有什么优先级的概念了，系统的实时性——也就是
按照优先级调度的能力就无法得到保障了。


Priority ceiling:

这是解决Priority inversion问题的一个方法，即将持有锁的任务提高到当时系统中的最高优先级，
这样的话，上面例子中的A与持有锁的C将具有同样的优先级，或者C的优先级将大于A的优先级，那么A就
无法将C从CPU上挤出去。从而保证锁的有效性。但是这仍然无法解决实时性的问题。


Linux内核中采用了priority ceiling的方法；就是这里的preempt_disable()。当系统禁止抢占时，
则系统中仅有一个优先级，于是上例中的C就取得了当时系统中的最高优先级；而A的优先级看上去也与C保持
了一致。


spinlock问什么说它会发生由优先级反转而带来的死锁:

因为spinlock比较特殊，不像其他锁机制，任务在遇到block情况是会进入睡眠状态而让出CPU使用权。
spinlock则会一直占有CPU做nop，这样导致CPU空转。在该任务用完时间片之后，才会让出CPU。比如上面
例子的任务A,它可能一直占有CPU而“忙等”，这就造成了死锁的可能。


```c

/* 单核 CPU */
#define __LOCK(lock) \
    do { preempt_disable(); __acquire(lock); (void)(lock); } while (0)

#define _raw_spin_lock(lock)  __LOCK(lock)

#define raw_spin_lock(lock)  _raw_spin_lock(lock)

/* 多核 CPU */
void __lockfunc _raw_spin_lock(raw_spinlock_t *lock)     __acquires(lock);

BUILD_LOCK_OPS(spin, raw_spinlock);

#ifdef CONFIG_INLINE_SPIN_LOCK
#define _raw_spin_lock(lock) __raw_spin_lock(lock)
#endif

static inline void __raw_spin_lock(raw_spinlock_t *lock)
{
    preempt_disable();
    spin_acquire(&lock->dep_map, 0, 0, _RET_IP_);
    LOCK_CONTENDED(lock, do_raw_spin_trylock, do_raw_spin_lock);
}



static inline void spin_lock(spinlock_t *lock)
{
    raw_spin_lock(&lock->rlock);
}

```
