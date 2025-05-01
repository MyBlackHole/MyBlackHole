# fs

```shell
❯ sudo bcachefs fs usage /run/media/black/Data
Filesystem: 81e71ef6-d3be-4b2d-bfee-ff49e14fe6a2
Size:                  1840366936576
Used:                   925918881280
Online reserved:             3311104

Data type       Required/total  Durability    Devices
reserved:       1/1                [] 97127424
btree:          1/1             1             [sda]            12834308096
user:           1/1             1             [sda]           912982564864

Btree usage:
extents:          2004615168
inodes:           6747586560
dirents:          1361575936
xattrs:               262144
alloc:             382205952
reflink:            23855104
subvolumes:           262144
snapshots:            262144
lru:                 2621440
freespace:           1048576
need_discard:         524288
backpointers:     2021130240
bucket_gens:         5505024
snapshot_trees:       262144
deleted_inodes:       262144
logged_ops:           524288
subvolume_children:   262144
accounting:        281542656

(no label) (device 0):           sda              rw
                                data         buckets    fragmented
  free:                1054442782720         2011190
  sb:                        3149824               7        520192
  journal:                4294967296            8192
  btree:                 12834308096           31418    3637772288
  user:                 912982564864         1764649   12201730048
  cached:                          0               0
  parity:                          0               0
  stripe:                          0               0
  need_gc_gens:                    0               0
  need_discard:              1048576               2
  unstriped:                       0               0
  capacity:            2000398843904         3815458
```
