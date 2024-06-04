# fs

```shell

❯ bcachefs fs usage /
Filesystem: da3120a3-5496-4be4-ba50-890bfd93db4f
Size:                   470152244224
Used:                    81383309312
Online reserved:             2778624

Data type       Required/total  Durability    Devices
reserved:       1/0              [] 4402471424
btree:          1/1             1             [nvme0n1p2]       1644167168
user:           1/1             1             [nvme0n1p2]      73114168320

(no label) (device 0):     nvme0n1p2              rw
                                data         buckets    fragmented
  free:                 426559406080         1627195
  sb:                        3149824              13        258048
  journal:                2147483648            8192
  btree:                  1644167168            6272
  user:                  73114168320          307771    7566152704
  cached:                          0               0
  parity:                          0               0
  stripe:                          0               0
  need_gc_gens:                    0               0
  need_discard:               262144               1
  capacity:             511035047936         1949444
```
