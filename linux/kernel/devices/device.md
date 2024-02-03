# device

| 字符设备             | 块设备                                        |
| -------------------- | --------------------------------------------- |
| 1byte                | 块，硬件块各有不同，但是内核都使用512byte描述 |
| 顺序访问             | 随机访问                                      |
| 没有缓存，实时操作   | 有缓存，不是实时操作                          |
| 一般提供接口给应用层 | 块设备一般提供接口给文件系统                  |
| 是被用户程序调用     | 由文件系统程序调用                            |

## 核心结构
- **gendisk**是一个物理磁盘或分区在内核中的描述
- **block_device_operations**描述磁盘的操作方法集，block_device_operations之于gendisk，类似于file_operations之于cdev
- **request_queue**对象表示针对一个gendisk对象的所有请求的队列，是相应gendisk对象的一个域
- **request**表示经过IO调度之后的针对一个gendisk(磁盘)的一个"请求"，是request_queue的一个节点。多个request构成了一个request_queue
- **bio**表示应用程序对一个gendisk(磁盘)原始的访问请求，一个bio由多个bio_vec，多个bio经过IO调度和合并之后可以形成一个request。 
- **bio_vec**描述的应用层准备读写一个gendisk(磁盘)时需要使用的内存页**page的一部分**，即上文中的"段"，多个bio_vec和bio_iter形成一个bio
- **bvec_iter**描述一个bio_vec中的一个sector信息
![[imgs/Pasted image 20230624181643.png]]
