## major与minor

首先，我们需要了解Linux下块设备的major与minor的概念。

对于Linux用户，可以尝试执行在自己的电脑上执行一下`ls -l /dev | grep -E 'nvme|sd`。输出大概会长这样：

```bash
crw------- 1 root root    250,   0 Mar  9 13:25 nvme0
brw-rw---- 1 root disk    259,   0 Mar  9 13:25 nvme0n1
brw-rw---- 1 root disk    259,   1 Mar  9 13:25 nvme0n1p1
brw-rw---- 1 root disk      8,   0 Mar  9 13:25 sda
brw-rw---- 1 root disk      8,   1 Mar  9 13:25 sda1
brw-rw---- 1 root disk      8,   2 Mar  9 13:25 sda2
brw-rw---- 1 root disk      8,  16 Mar  9 13:25 sdb
brw-rw---- 1 root disk      8,  17 Mar  9 13:25 sdb1
brw-rw---- 1 root disk      8,  32 Mar  9 13:25 sdc
brw-rw---- 1 root disk      8,  33 Mar  9 13:25 sdc1
brw-rw---- 1 root disk      8,  48 Mar  9 13:25 sdd
brw-rw---- 1 root disk      8,  49 Mar  9 13:25 sdd1
```

对于其中两列的数字，左边是major，右边是minor。

这些块设备文件可以通过`mknod`命令创建，这里我选取了BSD的Manual作为参考：

```auto
     major   The major device number is an integer number which tells the ker-
             nel which device driver entry point to use.

     minor   The minor device number tells the kernel which one of several
             similar devices the node corresponds to; for example, it may be a
             specific serial port or pty.
```

简单而言，major是内核该设备的请求应该发送给哪一个驱动，而minor则是对应的块设备在这个驱动下对应的设备。

例如我的机器上有从sda到sdd共4块SATA硬盘，而SATA硬盘的设备都是由同一驱动所创建的，因此他们共享相同的major，而具体到每块硬盘与每个分区，则由该驱动创建不同的minor来进行。

同时，Linux内核提供了一个major设备的列表，我们可以通过`cat /proc/devices`进行查看：

```bash
Character devices:
  1 mem
  4 /dev/vc/0
  4 tty
  4 ttyS
  5 /dev/tty
  5 /dev/console
  5 /dev/ptmx
  7 vcs
 10 misc
 13 input
 21 sg
 29 fb
128 ptm
136 pts
153 spi
180 usb
189 usb_device
247 aux
248 cec
249 hidraw
250 nvme
251 bsg
252 rtc
253 dax

Block devices:
  8 sd
  9 md
 11 sr
252 mdp
253 device-mapper
254 bcache
259 blkext
```

由此，我们可以确认我们编写块设备驱动的目标，通过内核模块注册一个块设备major，然后在其下创建对应的minor设备来完成块设备的读写。

## 在系统中注册major设备

这里我们可以采用`register_blkdev`这个函数，该函数定义如下：

```c
int register_blkdev (unsigned int major, const char * name);
```

其中，name是我们需要创建的名称，这也是我们在`/proc/devices`中可以得到的一个标识符，而`major`则是我们可以注册的major号。

register\_blkdev的返回值：

+   不成功，返回值<0，并返回对应的错误代码
+   成功
    +   major = 0，返回对应的major号
    +   major != 0, 返回0表示成功

同样，在内核模块退出时，我们也应该调用`unregister_blkdev`与之对应，参数与`register_blkdev`完全相同，这里不做过多介绍。

最后，我们就得到了一个注册major设备代码的雏形：

```cpp
#include <linux/module.h>
#include <linux/kernel.h>
#include <linux/blkdev.h>

MODULE_LICENSE("GPL");
MODULE_AUTHOR("cyy");
MODULE_DESCRIPTION("Simple Ramdisk Module");

int myramdisk_major = 0;

static int __init myramdisk_init(void) {
    myramdisk_major = register_blkdev(0,"myramdisk");
    if (myramdisk_major < 0) {
        printk(KERN_WARNING "register_blkdev myramdisk failed.\n");
    }
    else {
        printk(KERN_INFO "register_blkdev myramdisk succeed, major = %d.\n",myramdisk_major);
    }
    return 0;
}

static void __exit myramdisk_exit(void) {
    unregister_blkdev(myramdisk_major,"myramdisk");
}

module_init(myramdisk_init);
module_exit(myramdisk_exit);
```

## 注册一个minor设备

对于minor设备的注册，我们需要了解一个结构体是`sturct gendisk`。

我们可以通过调用函数`struct gendisk *alloc_disk(int minors)`（如果传递了第二参数，则第二参数为numa\_node，我们在PC上使用暂不考虑numa节点问题）来得到一个对应的结构体，该结构体定义如下：

```c
struct gendisk {
    /* major, first_minor and minors are input parameters only,
     * don't use directly.  Use disk_devt() and disk_max_parts().
     */
    int major;          /* major number of driver */
    int first_minor;
    int minors;                     /* maximum number of minors, =1 for
                                         * disks that can't be partitioned. */

    char disk_name[DISK_NAME_LEN];  /* name of major driver */

    unsigned short events;      /* supported events */
    unsigned short event_flags; /* flags related to event processing */

    /* Array of pointers to partitions indexed by partno.
     * Protected with matching bdev lock but stat and other
     * non-critical accesses use RCU.  Always access through
     * helpers.
     */
    struct disk_part_tbl __rcu *part_tbl;
    struct hd_struct part0;

    const struct block_device_operations *fops;
    struct request_queue *queue;
    void *private_data;

    int flags;
    unsigned long state;
#define GD_NEED_PART_SCAN       0
    struct rw_semaphore lookup_sem;
    struct kobject *slave_dir;

    struct timer_rand_state *random;
    atomic_t sync_io;       /* RAID */
    struct disk_events *ev;
#ifdef  CONFIG_BLK_DEV_INTEGRITY
    struct kobject integrity_kobj;
#endif  /* CONFIG_BLK_DEV_INTEGRITY */
#if IS_ENABLED(CONFIG_CDROM)
    struct cdrom_device_info *cdi;
#endif
    int node_id;
    struct badblocks *bb;
    struct lockdep_map lockdep_map;
};
```

可以看到，这里有一个很重要的结构体我们还没有定义，叫做`block_device_operations`，我们接着看内核的代码，发现该结构体定义如下：

```c
struct block_device_operations {
    blk_qc_t (*submit_bio) (struct bio *bio);
    int (*open) (struct block_device *, fmode_t);
    void (*release) (struct gendisk *, fmode_t);
    int (*rw_page)(struct block_device *, sector_t, struct page *, unsigned int);
    int (*ioctl) (struct block_device *, fmode_t, unsigned, unsigned long);
    int (*compat_ioctl) (struct block_device *, fmode_t, unsigned, unsigned long);
    unsigned int (*check_events) (struct gendisk *disk,
                      unsigned int clearing);
    void (*unlock_native_capacity) (struct gendisk *);
    int (*revalidate_disk) (struct gendisk *);
    int (*getgeo)(struct block_device *, struct hd_geometry *);
    int (*set_read_only)(struct block_device *bdev, bool ro);
    /* this callback is with swap_lock and sometimes page table lock held */
    void (*swap_slot_free_notify) (struct block_device *, unsigned long);
    int (*report_zones)(struct gendisk *, sector_t sector,
            unsigned int nr_zones, report_zones_cb cb, void *data);
    char *(*devnode)(struct gendisk *disk, umode_t *mode);
    struct module *owner;
    const struct pr_ops *pr_ops;
};
```

这两个结构体类似于一个抽象类，存放了各种抽象接口的指针。

## block\_device\_operation

这个结构体对于我们来说其实可以不需要定义，我们只需要实现一个该类的owner然后将指针放入gendisk类中即可。

```c
const struct block_device_operations myramdisk_fops = {
    .owner = THIS_MODULE,
};
```

## blk\_mq\_ops

根据查阅内核代码，该结构体定义如下：

```c
struct blk_mq_ops {
    /**
     * @queue_rq: Queue a new request from block IO.
     */
    blk_status_t (*queue_rq)(struct blk_mq_hw_ctx *,
                 const struct blk_mq_queue_data *);
    //余下部分省略
}
```

该结构体只需要定义一个request请求函数即可，因此我们可以先写出以下代码：

```c
static blk_status_t myramdisk_queue_rq(struct blk_mq_hw_ctx *hctx,const struct blk_mq_queue_data* bd) {
    // TODO
    return BLK_STS_OK;
}

static const struct blk_mq_ops myramdisk_mq_ops = {
    .queue_rq = myramdisk_queue_rq,
};
```

## 编写创建dev函数

首先我们需要先定义出`myramdisk_dev`结构体，我们首先思考，它需要一个指针来记录存放数据的内存空间，然后需要实现一个`request_queue`以及`blk_mq`在进行init时所需要的`blk_mq_tag_set`，同时，我们还需要一个`gendisk`类，以及一个`spinlock`来构建临界区。

最终我们的结构体定义如下：

```c
struct myramdisk_dev {
    unsigned char *data;
    struct request_queue *queue;
    struct gendisk *gd;
    struct blk_mq_tag_set tag_set;
} myramdisk_devs[myramdisk_ndev];
```

之后，根据相关资料，我们对这些结构体进行初始化：

```c
static void myramdisk_add_device(myramdisk_dev *dev,int first_minor) {
    //alloc memory for ramdisk data
    dev->data = vmalloc(myramdisk_dev_size);
    //init mq
    dev->queue = blk_mq_init_sq_queue(&dev->tag_set,&myramdisk_mq_ops,128,BLK_MQ_F_SHOULD_MERGE | BLK_MQ_F_SG_MERGE);
    blk_queue_logical_block_size(dev->queue,myramdisk_bs);
    dev->queue->queuedata = dev;
    //init gendisk
    dev->gd = alloc_disk(myramdisk_major);
    dev->gd->major = myramdisk_major;
    dev->gd->first_minor = first_minor;
    dev->gd->fops = &myramdisk_fops;
    dev->gd->private_data = dev;
    dev->gd->queue = dev->queue;
    sprintf(dev->gd->disk_name,"myramdisk%d",first_minor);//the filename in /dev/
    set_capacity(dev->gd,myramdisk_dev_size / KERNEL_SECTOR_SIZE);
    //add disk
    add_disk(dev->gd);
}
```

## kmalloc与vmalloc

这里需要介绍一下Linux内核模块编写中所用到的`kmalloc`和`vmalloc`，其中：

+   `kmalloc`用于分配一个小于页面大小的空间，比如一些小的结构体，它的特点是返回的虚拟地址在物理地址上是连续的。
+   `vmalloc`返回的虚拟地址在物理地址上是不连续的，经过分页管理，比较适合用于开辟一些较大的缓冲区使用。

## init与exit以及头文件的添加

由于我们创建了设备，init和exit也需要进行相应的修改。

在init中加入以下几行：

```c
int i;
for (i=0;i<myramdisk_ndev;i++) 
myramdisk_add_device(&myramdisk_devs[i],i*16);
```

在exit中加入以下几行：

```c
int i;
for (i=0;i<myramdisk_ndev;i++) {
    if (myramdisk_devs[i].data) vfree(myramdisk_devs[i].data);
    if (myramdisk_devs[i].queue) blk_cleanup_queue(myramdisk_devs[i].queue);
    if (myramdisk_devs[i].gd) {
        del_gendisk(myramdisk_devs[i].gd);
        put_disk(myramdisk_devs[i].gd);
    }
}
unregister_blkdev(myramdisk_major,"myramdisk");
```

此外，对于以上用到的函数与结构体，我们还需要添加这两个头文件

```c
#include <linux/blk-mq.h>
#include <linux/vmalloc.h>
```

## 继续编写request函数

如果此时我们直接编译然后insmod，会发现insmod一直卡住，原因是request函数并没有对队列进行正确的处理，这时候我们需要对该函数进行实现。

这里我们需要用到的一个宏是`rq_for_each_segment`，它对单个request的每个部分进行循环响应，类似于现代语言中的foreach的语法。

然后剩下的部分就非常容易理解了，如代码所示：

```c
static blk_status_t myramdisk_queue_rq(struct blk_mq_hw_ctx *hctx,const struct blk_mq_queue_data* bd) {
    blk_status_t ret = BLK_STS_OK;
    struct request *req = bd->rq;
    struct myramdisk_dev *dev = req->rq_disk->private_data;
    sector_t rq_pos = blk_rq_pos(req);
    blk_mq_start_request(req);

    struct bio_vec bvec;
    struct req_iterator iter;
    rq_for_each_segment(bvec, req, iter) {
        size_t num_sector = blk_rq_cur_sectors(req);
        unsigned char *buffer = page_address(bvec.bv_page) + bvec.bv_offset;
        unsigned long offset = rq_pos * KERNEL_SECTOR_SIZE;
        if ((offset + num_sector*KERNEL_SECTOR_SIZE) <= myramdisk_dev_size) {//avoid buffer overflow
            if (rq_data_dir(req) == WRITE) memcpy(dev->data+offset,buffer,num_sector*KERNEL_SECTOR_SIZE);
            else memcpy(buffer,dev->data+offset,num_sector*KERNEL_SECTOR_SIZE);
        }
        else {
            ret = BLK_STS_IOERR;
            goto end;
        }
        rq_pos += num_sector;
    }
end:
    blk_mq_end_request(req,ret);
    return ret;
}
```

## 成品

[https://github.com/cyyself/simple-linux-kernel-module/tree/master/ramdisk](https://github.com/cyyself/simple-linux-kernel-module/tree/master/ramdisk)

可在Linux 5.10.3内核上正常使用

## 参考资料

[https://prog.world/linux-kernel-5-0-we-write-simple-block-device-under-blk-mq/](https://prog.world/linux-kernel-5-0-we-write-simple-block-device-under-blk-mq/)

[https://github.com/martinezjavier/ldd3/blob/master/sbull/sbull.c](https://github.com/martinezjavier/ldd3/blob/master/sbull/sbull.c)