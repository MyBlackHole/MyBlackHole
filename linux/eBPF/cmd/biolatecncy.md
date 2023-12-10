# biolatecncy

磁盘 I/O 延迟的汇总分布

```shell
sudo /usr/share/bcc/tools/biolatecncy

Tracing block device I/O... Hit Ctrl-C to end.
     usecs               : count     distribution
         0 -> 1          : 0        |                                        |
         2 -> 3          : 0        |                                        |
         4 -> 7          : 0        |                                        |
         8 -> 15         : 0        |                                        |
        16 -> 31         : 0        |                                        |
        32 -> 63         : 0        |                                        |
        64 -> 127        : 0        |                                        |
       128 -> 255        : 3        |****************************************|
       256 -> 511        : 3        |****************************************|
       512 -> 1023       : 1        |*************                           |
```
