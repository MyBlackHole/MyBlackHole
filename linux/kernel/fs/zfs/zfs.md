# zfs 

动态文件系统

![[imgs/img_v2_df73041f-8561-466d-a96e-9c680c24987g.webp]]
![[imgs/Pasted image 20230813194000.png]]

## 概念

### spa
Storage Pool Allocator(存储池分配)

从内核提供的多个块设备中抽象出存储池的子系统。 SPA 进一步分为 ZIO 和 VDEV 两大部分和其余一些小的子系统。

SPA 对 DMU 提供的接口不同于传统的块设备接口，完全利用了 CoW 文件系统对写入位置不敏感的特点。 传统的块设备接口通常是写入时指定一个写入地址，把缓冲区写到磁盘指定的位置上，而 DMU 可以让 SPA 做两种操作：

1. `write` ， DMU 交给 SPA 一个数据块的内存指针， SPA 负责找设备写入这个数据块，然后返回给 DMU 一个 block pointer 。
2. `read` ，DMU 交给 SPA 一个 block pointer ，SPA 读取设备并返回给 DMU 完整的数据块。

也就是说，在 DMU 让 SPA 写数据块时， DMU 还不知道 SPA 会写入的地方，这完全由 SPA 判断， 这一点通常被称为 Write Anywhere ，在别的 CoW 文件系统比如 Btrfs 和 WAFL 中也有这个特点。 反过来 SPA 想要对一个数据块操作时，也完全不清楚这个数据块用于什么目的，属于什么文件或者文件系统结构

### vdev
Virtual DEVice (虚拟设备)

VDEV 在 ZFS 中的作用相当于 Linux 内核的 Device Mapper 层或者 FreeBSD GEOM 层，提供 Stripe/Mirror/RAIDZ 之类的多设备存储池管理和抽象。 ZFS 中的 vdev 形成一个树状结构，在树的底层是从内核提供的物理设备， 其上是虚拟的块设备。每个虚拟块设备对上对下都是块设备接口，除了底层的物理设备之外，位于中间层的 vdev 需要负责地址映射、容量转换等计算过程。

除了用于存储数据的 Stripe/Mirror/RAIDZ 之类的 VDEV ，还有一些特殊用途的 VDEV ，包括提供二级缓存的 L2ARC 设备，以及提供 ZIL 高速日志的 SLOG 设备

### zio
zfs io
作用相当于内核的 IO scheduler 和 pagecache write back 机制。 OpenZFS Summit 有个演讲整理了 ZIO 流水线的工作原理。 ZIO 内部使用流水线和事件驱动机制，避免让上层的 ZFS 线程阻塞等待在 IO 操作上。 ZIO 把一个上层的写请求转换成多个写操作，负责把这些写操作合并到 transaction group 提交事务组。 ZIO 也负责将读写请求按同步还是异步分成不同的读写优先级并实施优先级调度， 在 [OpenZFS 项目 wiki 页有一篇描述 ZIO 调度](https://github.com/zfsonlinux/zfs/wiki/ZIO-Scheduler) 的细节

除了调度之外， ZIO 层还负责在读写流水线中拆解和拼装数据块。上层 DMU 交给 SPA 的数据块有固定大小， 目前默认是 128KiB ，pool 整体的参数可以调整块大小在 4KiB 到 8MiB 之间。ZIO 拿到整块大小的数据块之后，在流水线中可以对数据块做诸如以下操作：

1. 用压缩算法，压缩/解压数据块。
2. 查询 dedup table ，对数据块去重。
3. 加密/解密数据块。
4. 计算数据块的校验和。
5. 如果底层分配器不能分配完整的 128KiB （或 zpool 设置的大小），那么尝试分配多个小块，然后用多个 512B 的指针间接块连起多个小块的，拼装成一个虚拟的大块，这个机制叫 [gang block](https://utcc.utoronto.ca/~cks/space/blog/solaris/ZFSGangBlocks) 。通常 ZFS 中用到 gang block 时，整个存储池处于极度空间不足的情况，由 gang block 造成严重性能下降，而 gang block 的意义在于在空间接近要满的时候也能 CoW 写入一些元数据，释放亟需的存储空间。

可见经过 ZIO 流水线之后，数据块不再是统一大小，这使得 ZFS 用在 4K 对齐的磁盘或者 SSD 上有了一些新的挑战

### MetaSlab
MetaSlab 是 ZFS 的块分配器。 VDEV 把存储设备抽象成存储池之后， MetaSlab 负责实际从存储设备上分配数据块，跟踪记录可用空间和已用空间。

叫 MetaSlab 这个名字是因为 Jeff 最初同时也给 Solaris 内核写过 [slab 分配器](https://en.wikipedia.org/wiki/Slab_allocation) ，一开始设计 SPA 的时候 Jeff 想在 SPA 中也利用 Solaris 的 slab 分配器对磁盘空间使用类似的分配算法。后来 MetaSlab 逐渐不再使用 slab 算法，只有名字留了下来。
MetaSlab 的结构很接近于 FreeBSD UFS 的 cylinder group ，或者 ext2/3/4 的 block group ，或者 xfs 的 allocation group ，目的都是让存储分配策略「局域化」， 或者说让近期分配的数据块的物理地址比较接近。在存储设备上创建 zpool 的时候，首先会尽量在存储设备上分配 200 个左右的 MetaSlab ，随后给 zpool 增加设备的话使用接近的 MetaSlab 大小。每个 MetaSlab 是连续的一整块空间，在 MetaSlab 内对数据块空间做分配和释放。磁盘中存储的 MetaSlab 的分配情况是按需载入内存的，系统 import zpool 时不需要载入所有 MetaSlab 到内存，而只需加载一小部分。当前载入内存的 MetaSlab 剩余空间告急时，会载入别的 MetaSlab 尝试分配，而从某个 MetaSlab 释放空间不需要载入 MetaSlab 。

OpenZFS Summit 也有一个对 MetaSlab 分配器性能的介绍，可以看到很多分配器内的细节。

### dsl
Dataset and Snapshot Layer(数据集与快照层)

负责创建和管理快照、克隆等数据集类型，跟踪它们的写入大小，最终删除它们。 由于 DMU 层面已经负责了对象的写时复制语义和对象集的概念，所以 DSL 层面不需要直接接触写文件之类来自 ZPL 的请求，无论有没有快照对 DMU 层面一样采用写时复制的方式修改文件数据。 不过在删除快照和克隆之类的时候，则需要 DSL 参与计算没有和别的数据集共享的数据块并且删除它们。

DSL 管理数据集时，也负责管理数据集上附加的属性。ZFS 每个数据集有个属性列表，这些用 ZAP 存储， DSL 则需要根据数据集的上下级关系，计算出继承的属性，最终指导 ZIO 层面的读写行为。

除了管理数据集， DSL 层面也提供了 zfs 中 send/receive 的能力。 ZFS 在 send 时从 DSL 层找到快照引用到的所有数据块，把它们直接发往管道，在 receive 端则直接接收数据块并重组数据块指针。 因为 DSL 提供的 send/receive 工作在 DMU 之上，所以在 DSL 看到的数据块是 DMU 的数据块，下层 SPA 完成的数据压缩、加密、去重等工作，对 DMU 层完全透明。所以在最初的 send/receive 实现中，假如数据块已经压缩，需要在 send 端经过 SPA 解压，再 receive 端则重新压缩。最近 ZFS 的 send/receive 逐渐打破 DMU 与 SPA 的壁垒，支持了直接发送已压缩或加密的数据块的能力。

### pool
[[storage_pool]] 存储池


### dataset
[[dataset]] 数据集

- 虚拟设备（virtual devices,vdev）
对于操作系统来说，VDEV 和传统的 RAID 阵列卡所呈现的raid设备类似
VDEV有几种不同的类型，每种类型都有自己的优势，包括冗余和速度
一般来说，VDEV的可靠性和安全性比阵列卡要好。因此使用ZFS时不建议使用阵列卡
让ZFS直接管理磁盘。

VDEV的类型
* stripe (条带。单个磁盘，没有冗余)
* mirror (镜像。支持n-way镜像)
* raidz
	* raidz1 (一个奇偶校验磁盘, 类似于RAID 5)
	* raidz2 (两个奇偶校验磁盘, 类似于RAID 6)
	* raidz3 (三个奇偶校验磁盘, 没有类似RAID等级)
* disk（磁盘）
* file (文件。不推荐在生产环境中使用，因为中间又多了一层不必要的文件系统)

数据会以条带方式存储于存储池中的所有VDEV上。因此一个存储池中的VDEV越多，IOPS就越高。

- 空间不足时写入性能下降
	- 因为没有大量连续空间,导致发生大量随机写入导致

> https://farseerfc.me/zhs/zfs-layered-architecture-design.html#spa
