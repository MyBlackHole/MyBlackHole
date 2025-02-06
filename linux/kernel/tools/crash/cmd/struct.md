# struct

查看数据结构
```shell
struct -o [struct] ： 显示结构体中成员的偏移
struct [struct] [address] ： 显示对应地址结构体的值
[结构][地址] ：简化形式显示对应地址结构体的值
[结构][地址] -xo： 打印结构体定义和大小
[struct].member[address]： 显示某个成员的值

[struct] -ox 打印结构体定义和十六进制大小
```

- 查看结构体的定义
```shell
struct file

struct file {
    union {
        struct llist_node fu_llist;
        struct callback_head fu_rcuhead;
    } f_u;
    struct path f_path;
    struct inode *f_inode;
    const struct file_operations *f_op;
    spinlock_t f_lock;
    enum rw_hint f_write_hint;
    atomic_long_t f_count;
    unsigned int f_flags;
    fmode_t f_mode;
    struct mutex f_pos_lock;
    loff_t f_pos;
    struct fown_struct f_owner;
    const struct cred *f_cred;
    struct file_ra_state f_ra;
    u64 f_version;
    void *f_security;
    void *private_data;
    struct list_head f_ep_links;
    struct list_head f_tfile_llink;
    struct address_space *f_mapping;
    errseq_t f_wb_err;
}


crash> struct -o file
struct file {
        union {
            struct llist_node fu_llist;
            struct callback_head fu_rcuhead;
    [0] } f_u;
   [16] struct path f_path;
   [32] struct inode *f_inode;
   [40] const struct file_operations *f_op;
   [48] spinlock_t f_lock;
   [52] enum rw_hint f_write_hint;
   [56] atomic_long_t f_count;
   [64] unsigned int f_flags;
   [68] fmode_t f_mode;
   [72] struct mutex f_pos_lock;
  [104] loff_t f_pos;
  [112] struct fown_struct f_owner;
  [144] const struct cred *f_cred;
  [152] struct file_ra_state f_ra;
  [184] u64 f_version;
  [192] void *f_security;
  [200] void *private_data;
  [208] struct list_head f_ep_links;
  [224] struct list_head f_tfile_llink;
  [240] struct address_space *f_mapping;
  [248] errseq_t f_wb_err;
}
SIZE: 256



crash> struct file -o ffff80219de89200
struct file {
        union {
            struct llist_node fu_llist;
            struct callback_head fu_rcuhead;
  [ffff80219de89200] } f_u;
  [ffff80219de89210] struct path f_path;
  [ffff80219de89220] struct inode *f_inode;
  [ffff80219de89228] const struct file_operations *f_op;
  [ffff80219de89230] spinlock_t f_lock;
  [ffff80219de89234] enum rw_hint f_write_hint;
  [ffff80219de89238] atomic_long_t f_count;
  [ffff80219de89240] unsigned int f_flags;
  [ffff80219de89244] fmode_t f_mode;
  [ffff80219de89248] struct mutex f_pos_lock;
  [ffff80219de89268] loff_t f_pos;
  [ffff80219de89270] struct fown_struct f_owner;
  [ffff80219de89290] const struct cred *f_cred;
  [ffff80219de89298] struct file_ra_state f_ra;
  [ffff80219de892b8] u64 f_version;
  [ffff80219de892c0] void *f_security;
  [ffff80219de892c8] void *private_data;
  [ffff80219de892d0] struct list_head f_ep_links;
  [ffff80219de892e0] struct list_head f_tfile_llink;
  [ffff80219de892f0] struct address_space *f_mapping;
  [ffff80219de892f8] errseq_t f_wb_err;
}

crash> struct atomic_long_t -o ffff80219de89238
typedef struct {
  [ffff80219de89238] long counter;
} atomic_long_t;
SIZE: 8

```

```shell
crash> struct file ffffa0282852cf00
struct file {
  f_u = {
    fu_llist = {
      next = 0x0
    },
    fu_rcuhead = {
      next = 0x0,
      func = 0x0
    }
  },
  f_path = {
    mnt = 0xffff804005bbdb20,
    dentry = 0xffff8022614235f0
  },
  f_inode = 0xffff80217999b938,
  f_op = 0xffff2f2ec07986a8,
  f_lock = {
    {
      rlock = {
        raw_lock = {
          {
            val = {
              counter = 0
            },
            {
              locked = 0 '\000',
              pending = 0 '\000'
            },
            {
              locked_pending = 0,
              tail = 0
            }
          }
        }
      }
    }
  },
  f_write_hint = WRITE_LIFE_NOT_SET,
  f_count = {
    counter = 1
  },
  f_flags = 132097,
  f_mode = 136151070,
  f_pos_lock = {
    owner = {
      counter = 0
    },
    wait_lock = {
      {
        rlock = {
          raw_lock = {
            {
              val = {
                counter = 0
              },
              {
                locked = 0 '\000',
                pending = 0 '\000'
              },
              {
                locked_pending = 0,
                tail = 0
              }
            }
          }
        }
      }
    },
    osq = {
      tail = {
        counter = 0
      }
    },
    wait_list = {
      next = 0xffffa0282852cf58,
      prev = 0xffffa0282852cf58
    }
  },
  f_pos = 204,
  f_owner = {
    lock = {
      raw_lock = {
        {
          cnts = {
            counter = 0
          },
          {
            wlocked = 0 '\000',
            __lstate = "\000\000"
          }
        },
        wait_lock = {
          {
            val = {
              counter = 0
            },
            {
              locked = 0 '\000',
              pending = 0 '\000'
            },
            {
              locked_pending = 0,
              tail = 0
            }
          }
        }
      }
    },
    pid = 0x0,
    pid_type = PIDTYPE_PID,
    uid = {
      val = 0
    },
    euid = {
      val = 0
    },
    signum = 0
  },
  f_cred = 0xffff8020756f4180,
  f_ra = {
    start = 0,
    size = 0,
    async_size = 0,
    ra_pages = 1024,
    mmap_miss = 0,
    prev_pos = -1
  },
  f_version = 0,
  f_security = 0x0,
  private_data = 0x0,
  f_ep_links = {
    next = 0xffffa0282852cfd0,
    prev = 0xffffa0282852cfd0
  },
  f_tfile_llink = {
    next = 0xffffa0282852cfe0,
    prev = 0xffffa0282852cfe0
  },
  f_mapping = 0xffff80217999baa8,
  f_wb_err = 0
}
```


- 查看结构体的成员变量
```shell
dentry -l dentry.d_iname 0xffff80226b0b4900
```

- 根据结构体成员变量地址获取结构体信息
```shell
# 0xffffffffb9b29460 是 kmem_cache 结构体的 list 成员地址
crash> struct kmem_cache -ol kmem_cache.list 0xffffffffb9b29460
struct kmem_cache {
  [ffffffffb9b29400] struct kmem_cache_cpu *cpu_slab;
  [ffffffffb9b29408] slab_flags_t flags;
  [ffffffffb9b29410] unsigned long min_partial;
  [ffffffffb9b29418] unsigned int size;
  [ffffffffb9b2941c] unsigned int object_size;
  [ffffffffb9b29420] unsigned int offset;
  [ffffffffb9b29424] unsigned int cpu_partial;
  [ffffffffb9b29428] struct kmem_cache_order_objects oo;
  [ffffffffb9b2942c] struct kmem_cache_order_objects max;
  [ffffffffb9b29430] struct kmem_cache_order_objects min;
  [ffffffffb9b29434] gfp_t allocflags;
  [ffffffffb9b29438] int refcount;
  [ffffffffb9b29440] void (*ctor)(void *);
  [ffffffffb9b29448] unsigned int inuse;
  [ffffffffb9b2944c] unsigned int align;
  [ffffffffb9b29450] unsigned int red_left_pad;
  [ffffffffb9b29458] const char *name;
  [ffffffffb9b29460] struct list_head list;
  [ffffffffb9b29470] struct kobject kobj;
  [ffffffffb9b294d0] struct work_struct kobj_remove_work;
  [ffffffffb9b29510] struct memcg_cache_params memcg_params;
  [ffffffffb9b29588] unsigned int max_attr_size;
  [ffffffffb9b29590] struct kset *memcg_kset;
  [ffffffffb9b29598] unsigned int remote_node_defrag_ratio;
  [ffffffffb9b295a0] unsigned int *random_seq;
  [ffffffffb9b295a8] unsigned int useroffset;
  [ffffffffb9b295ac] unsigned int usersize;
  [ffffffffb9b295b0] struct kmem_cache_node *node[1024];
}
SIZE: 8624
```
