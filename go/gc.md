### gc
垃圾回收器

### 常见的垃圾回收机制
1. 引用计数
对每个对象维护一个引用计数，当引用对象的对象被销毁时，引用计数-1，如果引用计数为0，则进行垃圾回收
- 优点：对象可以很快的被回收，不会出现内存耗尽或达到某个阀值时才回收。
- 缺点：不能很好的处理循环引用，而且实时维护引用计数，有也一定的代价。
- 代表语言：Python、PHP、Swift
 
2. 标记-清除
从根变量开始遍历所有引用的对象，引用的对象标记为"被引用"，没有被标记的进行回收
- 优点：解决了引用计数的缺点。
- 缺点：需要STW，即要暂时停掉程序运行。
- 代表语言：Golang(其采用三色标记法)

3. 分代收集
按照对象生命周期长短划分不同的代空间，生命周期长的放入老年代，而短的放入新生代，不同代有不能的回收算法和回收频率。
- 优点：回收性能好
- 缺点：算法复杂
- 代表语言： JAVA

### golang 的标记清除
如下图所示，通过gcmarkBits位图标记span的块是否被引用。对应内存分配中的bitmap区
1. 三色标记
- 灰色：对象已被标记，但这个对象包含的子对象未标记
- 黑色：对象已被标记，且这个对象包含的子对象也已标记，gcmarkBits对应的位为1（该对象不会在本次GC中被清理）
- 白色：对象未被标记，gcmarkBits对应的位为0（该对象将会在本次GC中被清理）
例如
    当前内存中有A~F一共6个对象，根对象a,b本身为栈上分配的局部变量，根对象a、b分别引用了对象A、B, 而B对象又引用了对象D，则GC开始前各对象的状态如下图所示:
    - 初始状态下所有对象都是白色的。
    - 接着开始扫描根对象a、b; 由于根对象引用了对象A、B,那么A、B变为灰色对象，接下来就开始分析灰色对象，分析A时，A没有引用其他对象很快就转入黑色，B引用了D，则B转入黑色的同时还需要将D转为灰色，进行接下来的分析。
    - 灰色对象只有D，由于D没有引用其他对象，所以D转入黑色。标记过程结束
    - 最终，黑色的对象会被保留下来，白色对象会被回收掉。

2. GC的触发
- 阈值：默认内存扩大一倍，启动gc
- 定期：默认2min触发一次gc，src/runtime/proc.go:forcegcperiod
- 手动：runtime.gc()

3. STW
stop the world是gc的最大性能问题，对于gc而言，需要停止所有的内存变化，即停止所有的goroutine，等待gc结束之后才恢复。
标记-清除(mark and sweep)算法的STW(stop the world)操作，就是runtime把所有的线程全部冻结掉，所有的线程全部冻结意味着用户逻辑是暂停的。这样所有的对象都不会被修改了，这时候去扫描是绝对安全的。
Go如何减短这个过程呢？标记-清除(mark and sweep)算法包含两部分逻辑：标记和清除。
我们知道Golang三色标记法中最后只剩下的黑白两种对象，黑色对象是程序恢复后接着使用的对象，如果不碰触黑色对象，只清除白色的对象，肯定不会影响程序逻辑。所以： 清除操作和用户逻辑可以并发。
标记操作和用户逻辑也是并发的，用户逻辑会时常生成对象或者改变对象的引用，那么标记和用户逻辑如何并发呢？

4. GC流程
- Sweep Termination: 对未清扫的span进行清扫, 只有上一轮的GC的清扫工作完成才可以开始新一轮的GC
- Mark: 扫描所有根对象, 和根对象可以到达的所有对象, 标记它们不被回收
- Mark Termination: 完成标记工作, 重新扫描部分根对象(要求STW)
- Sweep: 按标记结果清扫span
目前整个GC流程会进行两次STW(Stop The World), 第一次是Mark阶段的开始, 第二次是Mark Termination阶段.

- 第一次STW会准备根对象的扫描, 启动写屏障(Write Barrier)和辅助GC(mutator assist).
- 第二次STW会重新扫描部分根对象, 禁用写屏障(Write Barrier)和辅助GC(mutator assist).
需要注意的是, 不是所有根对象的扫描都需要STW, 例如扫描栈上的对象只需要停止拥有该栈的G.
从go 1.9开始, 写屏障的实现使用了Hybrid Write Barrier, 大幅减少了第二次STW的时间.

5. 写屏障
因为go支持并行GC， GC的扫描和go代码可以同时运行，这样带来的问题是GC扫描的过程中go代码有可能改变了对象的依赖树。
例如开始扫描时发现根对象A和B，B拥有C的指针。
    - GC先扫描A，A放入黑色
    - B把C的指针交给A
    - GC再扫描B，B放入黑色
    - C在白色，会回收；但是A其实引用了C。
为了避免这个问题, go在GC的标记阶段会启用写屏障(Write Barrier).
启用了写屏障(Write Barrier)后，
    - GC先扫描A，A放入黑色
    - B把C的指针交给A
    - 由于A在黑色，所以C放入灰色
    - C没有子对象，放入黑色
    - 扫描B，B没有子对象，放入黑色
即使A可能会在稍后丢掉C, 那么C就在下一轮回收。
开启写屏障之后，当指针发生改变, GC会认为在这一轮的扫描中这个指针是存活的, 所以放入灰色。


### 标准进程的内存模型 (有问题)
- code area (方法区)
- static area (静态变量)
- heap (堆)
- stack (栈)
![[imgs/Pasted image 20230303222000.png]]
