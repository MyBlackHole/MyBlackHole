# test

```bash
# 使用 zfs vol 测试
sudo zfs create -V 1G xfs_test/xfs_test_1

sudo mkfs.xfs /dev/xfs_test/xfs_test_1
specified blocksize 4096 is less than device physical sector size 8192
switching to logical sector size 512
meta-data=/dev/xfs_test/xfs_test_1 isize=512    agcount=4, agsize=65536 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=1, sparse=1, rmapbt=0
         =                       reflink=1    bigtime=1 inobtcount=1 nrext64=0
data     =                       bsize=4096   blocks=262144, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0, ftype=1
log      =internal log           bsize=4096   blocks=16384, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =无                    extsz=4096   blocks=0, rtextents=0
Discarding blocks...Done.

# 碎片化状态
sudo xfs_db -c frag -r /dev/xfs_test/xfs_test_1
actual 0, ideal 0, fragmentation factor 0.00%
Note, this number is largely meaningless.
Files on this filesystem average -nan extents per file

mount /dev/zd0
sudo xfs_db /dev/zd0
xfs_db> p
magicnum = 0x58465342 (这个就是XFSB的acsii码)
blocksize = 4096 (逻辑块大小)
dblocks = 262144 (磁盘总块数，块数*块大小就是磁盘的空间了)
rblocks = 0
rextents = 0
uuid = fa25581c-61b2-43f0-af5e-557401fd8ae6
logstart = 131078
rootino = 128 (根节点inode号)
rbmino = 129
rsumino = 130
rextsize = 1
agblocks = 65536 (每个AG的块数量)
agcount = 4 (4个AG，和AG块数乘起来就是块总数)
rbmblocks = 0
logblocks = 16384 (日志块数)
versionnum = 0xb4a5
sectsize = 512
inodesize = 512
inopblock = 8 (每个block可以存储的inode的个数，4096/512=8)
fname = "\000\000\000\000\000\000\000\000\000\000\000\000"
blocklog = 12 (2的12次方，这个可以和page的shift类比)
sectlog = 9 (sector大小的log表示)
inodelog = 9 (inode大小的log表示)
inopblog = 3 (每个block可以存储的inode的log表示)
agblklog = 16 (每个ag可以管理的blcok个数的log表示，这个存在向上取整)
rextslog = 0
inprogress = 0
imax_pct = 25
icount = 64
ifree = 60
fdblocks = 213649
frextents = 0
uquotino = null
gquotino = null
qflags = 0
flags = 0
shared_vn = 0
inoalignmt = 8
unit = 0
width = 0
dirblklog = 0
logsectlog = 0
logsectsize = 0
logsunit = 1
features2 = 0x18a
bad_features2 = 0x18a
features_compat = 0
features_ro_compat = 0xd
features_incompat = 0xb
features_log_incompat = 0
crc = 0xefac30f0 (correct)
spino_align = 4
pquotino = null
lsn = 0x10000000f
meta_uuid = 00000000-0000-0000-0000-000000000000
```
