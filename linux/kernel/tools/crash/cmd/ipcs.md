# ipcs

系统中使用共享内存的情况

```shell
crash> ipcs
SHMID_KERNEL     KEY      SHMID      UID   PERMS BYTES      NATTCH STATUS
ffff95ebc26f0400 00000000 196608     0     600   88         2
ffff95ebc26f3300 00000000 229377     0     600   16384      2
ffff95ebc26f3a00 00000000 262146     0     600   280        2
ffff953c6e347000 1100b30c 294915     0     666   160        0
ffff953c6e347600 1400b30b 327684     0     666   1570816    0

SEM_ARRAY        KEY      SEMID      UID   PERMS NSEMS
ffff95ebc8d37600 000000a7 98304      0     600   1

MSG_QUEUE        KEY      MSQID      UID   PERMS USED-BYTES   MESSAGES
(none allocated)
```
