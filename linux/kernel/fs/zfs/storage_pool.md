# storage pool 

ZFS 使用存储池来作为底层存储提供者（VDEV）的抽象。这样可以把用户可见的文件系统和底层的物理磁盘
布局分离开来。

## 命令

Actions: （存储池操作） 
* List   （列举）
* Status （查看状态）
* Destroy （删除）
* Get/Set properties （获取/设置属性）

### List zpools （列举存储池（也叫zpool））

```bash
# 创建一个raidz类型的存储池(名称为bucket）
$ zpool create bucket raidz1 gpt/zfs0 gpt/zfs1 gpt/zfs2
# 列出所有存储池
$ zpool list
NAME    SIZE  ALLOC   FREE  EXPANDSZ   FRAG    CAP  DEDUP  HEALTH  ALTROOT
zroot   141G   106G  35.2G         -    43%    75%  1.00x  ONLINE  -
# 列出某一存储池的详细信息
$ zpool list -v zroot
NAME                                     SIZE  ALLOC   FREE  EXPANDSZ   FRAG    CAP  DEDUP HEALTH  ALTROOT
zroot                                    141G   106G  35.2G         -    43%    75%  1.00x ONLINE  -
  gptid/c92a5ccf-a5bb-11e4-a77d-001b2172c655   141G   106G  35.2G         -    43%    75%
```

### Status of zpools （存储池状态）

```bash
# 获取全部zpool状态信息
$ zpool status
  pool: zroot
 state: ONLINE
  scan: scrub repaired 0 in 2h51m with 0 errors on Thu Oct  1 07:08:31 2015
config:
        NAME                                          STATE     READ WRITE CKSUM
        zroot                                         ONLINE       0     0     0
          gptid/c92a5ccf-a5bb-11e4-a77d-001b2172c655  ONLINE       0     0     0
errors: No known data errors
# 用scrub来更正存储池错误信息
$ zpool scrub zroot
$ zpool status -v zroot
  pool: zroot
 state: ONLINE
  scan: scrub in progress since Thu Oct 15 16:59:14 2015
        39.1M scanned out of 106G at 1.45M/s, 20h47m to go
        0 repaired, 0.04% done
config:
        NAME                                          STATE     READ WRITE CKSUM
        zroot                                         ONLINE       0     0     0
          gptid/c92a5ccf-a5bb-11e4-a77d-001b2172c655  ONLINE       0     0     0
errors: No known data errors
```

### Properties of zpools （存储池属性）

```bash
# 获取某一存储池的全部属性。属性可能是系统提供，也可能是用户设置
$ zpool get all zroot
NAME   PROPERTY                       VALUE                          SOURCE
zroot  size                           141G                           -
zroot  capacity                       75%                            -
zroot  altroot                        -                              default
zroot  health                         ONLINE                         -
...
# 设置存储池属性，下例这是设置comment(备注)属性
$ zpool set comment="Storage of mah stuff" zroot
$ zpool get comment
NAME   PROPERTY  VALUE                 SOURCE
tank   comment   -                     default
zroot  comment   Storage of mah stuff  local
```

### Remove zpool （删除存储池）

```bash
$ zpool destroy test
```


fdisk[[fdisk]] 分区工具
使用详情[https://www.escapelife.site/posts/caf259ea.html#toc-heading-9]

- 创建池
```shell
zpool create -f aiopool sdb # 会自动挂载到根目录 aiopool 目录

# RAID0 (数据条带化后，支持并行访问，提高读取速度)
zpool create -f aiopool sdb sda

# RAIO1 (镜像,用一半空间用于复制)
zpool create -f aiopool mirror sdb sda

# RAIO5/RAIOZ1 (驱动器至少是3个)
zpool create -f aiopool raidz1 sdb sda sdc

# RAID6/RAIDZ2 (驱动器至少是4个)
zpool create -f aiopool raidz2 sdb sda sdc sdd

# RAID10 (条带化镜像)
zpool create -f aiopool mirror sdb sda mirror sdc sdd
```

- 销毁池
```shell
zpool destroy aiopool
```

- 查询池状态
```shell
zpool status
zpool status -x
zpool list
zpool get all

# 对应信息解释
$ sudo zpool status
  pool: aiopool   # 池的名称
 state: ONLINE  # 池的当前运行状况
  scan: scrub repaired 0 in 0h39m with 0 errors on Sun Dec  8 01:03:48 2023

config: # 发出读取请求时出现I/O错误|发出写入请求时出现I/O错误|校验和错误
  NAME        STATE     READ WRITE CKSUM
aiopool     ONLINE       0     0     0
  sdb       ONLINE       0     0     0

errors: No known data errors  # 确定是否存在已知的数据错误
```

- 池修改历史
```shell
zpool history
```

- 更新指定池
```shell
zpool upgrade aiopool
```

- 更新所有池
```shell
zpool upgrade -a
```

- 添加硬盘
```shell
zpool add aiopool -f sda
```

- 删除硬盘
```shell
zpool remove aiopool sda
```

