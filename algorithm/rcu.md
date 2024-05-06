# rcu

Read，Copy-Update的缩写，意指读-复制更新
是一种同步机制。其将同步开销的非对称分布发挥到逻辑极限

[细节](http://www.wowotech.net/kernel_synchronization/rcu_fundamentals.html)

使用RCU注意如下事项：
* RCU适用多读少写场景。RCU和读写锁相似.但RCU的读者占锁没有任何的系统开销。写者与写者之间必须要保持同步,且写者必须要等它之前的读者全部都退出之后才能释放之前的资源。
* RCU保护的是指针.这一点尤其重要.因为指针赋值是一条单指令.也就是说是一个原子操作.因它更改指针指向没必要考虑它的同步.只需要考虑cache的影响；
* 读者是嵌套。也就是说rcu_read_lock()可以嵌套调用；
* 读者在持有rcu_read_lock()的时候,不能发生进程上下文切换.否则,因为写者需要要等待读者完成,写者进程也会一直被阻塞.
* 因为在非抢占场景中上下文切换不能发生在RCU的读侧临界区，所以已阻塞的任何线程必须在RCU读侧临界区之前完成。
* 任何没有跑在RCU读侧临界区的线程不能持有任何RCU受保护的引用
* 阻塞的线程不能持有受保护的RCU数据结构
* 线程不能引用已经删除的的受保护的RCU数据结构
* 阻塞的线程在受保护的RCU指针被移除后，不能再引用
* 从RCU保护的数据结构中删除给定元素后，一旦观察到所有线程处于阻塞状态，则RCU读侧临界区中的任何线程都无法持有该元素的引用

公共数据访问的同步机制：
* 互斥锁：保证同一时刻只有一个线程可以访问共享数据，适用于读写场景
* 条件变量：允许一个线程等待另一个线程完成某个操作，适用于写多读少场景
* 信号量：允许多个线程同时访问共享数据，适用于读多写少场景
    需要进行进程上下文切换，获取锁失败常见开销较大
* 自旋锁：允许多个线程同时访问共享数据，适用于读少写多场景
    持续持有自旋锁会导致CPU资源的浪费
* 读写锁：允许多个线程同时读共享数据，但只允许一个线程写共享数据，适用于读多写少场景
    对于写多读少场景，性能非常差，因为写者线程必须等待读者线程全部退出。
* RCU：允许多个线程同时读共享数据，但只允许一个线程写共享数据，适用于读多写少场景

## 原理
RCU是一种基于软件的同步机制，其通过延迟释放内存，使得读者线程可以访问最新的数据，而写者线程可以安全地更新数据。
RCU的基本思想是：写者线程将数据更新后，通过调用rcu_assign_pointer()函数将更新后的指针赋值给一个RCU指针。
读者线程通过调用rcu_dereference()函数读取这个指针，这个函数会阻塞直到写者线程完成更新。
[](https://www.readfog.com/a/1697544789531136000)

## API
```c
rcu_read_lock()
rcu_read_unlock()
synchronize_rcu()/call_rcu()
rcu_assign_pointer()
rcu_dereference()
```

## 读侧临界区 (read-side critical sections)
RCU读者执行的区域
每一个临界区开始于rcu_read_lock()
结束于rcu_read_unlock()
可能包含rcu_dereference()等访问RCU保护的数据结构的函数
这些指针函数实现了依赖顺序加载的概念，称为memory_order_consume加载。


## 写侧临界区
为适应读侧临界区，写侧推迟销毁并维护多个版本的数据结构
有大量的同步开销。此外，编写者必须使用某种同步机制（例如锁定）来提供有序的更新。


## 静默态(quiescent state)
当一个线程没有运行在读侧临界区时，其就处在静默状态
持续相当长一段时间的静默状态称之为延长的静默态（extended quiescent state）。


## 宽限期（Grace period)
宽限期是指所有线程都至少一次进入静默态的时间
宽限期前所有在读侧临界区的读者在宽限区后都会结束。不同的宽限期可能有部分或全部重叠。

## 实现
```c
struct rcu_head {
    struct rcu_head *next;
    void (*func)(void *arg);
    void *arg;
};
static struct rcu_head *rcu_freelist;

void call_rcu(struct rcu_head *head, void (*func)(void *arg), void *arg) {
    head->func = func;
    head->arg = arg;
    head->next = rcu_freelist;
    rcu_freelist = head;
}

void synchronize_rcu(void) {
    struct rcu_head *head, *next;
    head = rcu_freelist;
    rcu_freelist = NULL;
    while (head) {
        next = head->next;
        head->func(head->arg);
        head->func = NULL;
        head->arg = NULL;
        free(head);
        head = next;
    }
}

```
