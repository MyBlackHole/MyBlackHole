# buddy

伙伴系统是一种内存分配算法，
它通过维护一个大小为2的幂次方的空闲链表来管理内存。

echo 1 > /proc/sys/vm/drop_caches 
该命令会清除页面缓存,释放被缓存的数据所占用的内存,减少内碎片。

echo 2 > /proc/sys/vm/drop_caches
该命令会清除页面缓存和目录项缓存,释放更多内存,进一步整理内存碎片。

echo 3 > /proc/sys/vm/drop_caches
该命令会清除页面缓存、目录项缓存和inode缓存,彻底清空缓存占用的内存,最大限度减少内存碎片。


```shell
❯ cat /proc/buddyinfo
Node 0, zone      DMA      4      3      4      0      0      0      0      0      0      0      0
Node 0, zone    DMA32    258    253    133    122     89     48      9      4      4      8      6
Node 0, zone   Normal   100   52   3275   1814    891    185      2      5      3      0      1

分成11个块链表，分别对应为1、2、4、8、16、32、64、128、256、512、1024个页块

以Normal区域进行分析，第二列值为 100，表示当前系统中normal区域，可用的连续两页的内存数量为 100, 总大小为 100*2^1*PAGE_SIZE；
第三列值为52，表示当前系统中normal区域，可用的连续四页的内存数量为 52, 总大小为 52*2^2*PAGE_SIZE



❯ sudo cat /proc/pagetypeinfo
Page block order: 9
Pages per block:  512

Free pages count per migrate type at order       0      1      2      3      4      5      6      7      8      9     10
Node    0, zone      DMA, type    Unmovable      4      3      4      0      0      0      0      0      0      0      0
Node    0, zone      DMA, type      Movable      0      0      0      0      0      0      0      0      0      0      0
Node    0, zone      DMA, type  Reclaimable      0      0      0      0      0      0      0      0      0      0      0
Node    0, zone      DMA, type   HighAtomic      0      0      0      0      0      0      0      0      0      0      0
Node    0, zone      DMA, type          CMA      0      0      0      0      0      0      0      0      0      0      0
Node    0, zone      DMA, type      Isolate      0      0      0      0      0      0      0      0      0      0      0
Node    0, zone    DMA32, type    Unmovable     96    134     97    101     66     33      2      1      1      2      0
Node    0, zone    DMA32, type      Movable    162    118     35     19     22     14      7      3      2      4      7
Node    0, zone    DMA32, type  Reclaimable      0      0      0      1      1      0      1      0      1      1      0
Node    0, zone    DMA32, type   HighAtomic      0      0      0      0      0      0      0      0      0      0      0
Node    0, zone    DMA32, type          CMA      0      0      0      0      0      0      0      0      0      0      0
Node    0, zone    DMA32, type      Isolate      0      0      0      0      0      0      0      0      0      0      0
Node    0, zone   Normal, type    Unmovable    570    680    634    278    120     62      1      0      0      0      0
Node    0, zone   Normal, type      Movable      0    703   2521   1451    725    104      0      0      1      0      1
Node    0, zone   Normal, type  Reclaimable     27     13     17      0      6      3      0      0      1      0      0
Node    0, zone   Normal, type   HighAtomic      4     22     42     56     38     16      1      4      1      0      0
Node    0, zone   Normal, type          CMA      0      0      0      0      0      0      0      0      0      0      0
Node    0, zone   Normal, type      Isolate      0      0      0      0      0      0      0      0      0      0      0

Number of blocks type     Unmovable      Movable  Reclaimable   HighAtomic          CMA      Isolate
Node 0, zone      DMA            2            6            0            0            0            0
Node 0, zone    DMA32           37         1557            2            0            0            0
Node 0, zone   Normal         1411         4591          241           23            0            0
```
