# ipcs

- 查看系统使用的IPC共享内存资源
```shell
ipcs -m

------ Shared Memory Segments --------
key        shmid      owner      perms      bytes      nattch     status
0x00000000 23         black      600        524288     2          dest
0x6600af68 26         black      0          4096       0
0x66210001 30         black      0          4096       0

其中：
第一列就是共享内存的key；
第二列是共享内存的编号shmid；
第三列就是创建的用户owner；
第四列就是权限perms；
第五列为创建的大小bytes；
第六列为连接到共享内存的进程数nattach；
第七列是共享内存的状态status。
    其中显示“dest”表示共享内存段已经被删除，但是还有用户在使用它，当该段内存的mode字段设置为 SHM_DEST时就会显示“dest”。
    当用户调用shmctl的IPC_RMID时，内存先查看多少个进程与这个内存关联着，如果关联数为0，就会销 毁这段共享内存，否者设置这段内存的mod的mode位为SHM_DEST，如果所有进程都不用则删除这段共享内存。
```

- 查看系统使用的IPC队列资源
```shell
ipcs -q 查看系统使用的IPC队列资源
```

- 查看系统使用的IPC信号量资源
```shell
ipcs -s 查看系统使用的IPC信号量资源
```

- 删除共享内存：`ipcrm -m id`
```shell
❯ ipcs -m

------ Shared Memory Segments --------
key        shmid      owner      perms      bytes      nattch     status
0x00000000 23         black      600        524288     2          dest
0x6600af68 26         black      0          4096       0
0x66210001 30         black      0          4096       0

❯ ipcrm -m 26
❯ ipcs -m

------ Shared Memory Segments --------
key        shmid      owner      perms      bytes      nattch     status
0x00000000 23         black      600        524288     2          dest
0x66210001 30         black      0          4096       0
```

- 查询共享内存限制
```shell
ipcs -l
------ Messages Limits --------
max queues system wide = 32000
max size of message (bytes) = 8192
default max size of queue (bytes) = 16384

------ Shared Memory Limits --------
max number of segments = 4096
max seg size (kbytes) = 18014398509465599
max total shared memory (kbytes) = 18446744073709551612
min seg size (bytes) = 1

------ Semaphore Limits --------
max number of arrays = 32000
max semaphores per array = 32000
max semaphores system wide = 1024000000
max ops per semop call = 500
semaphore max value = 32767
```

- 限制共享内存的大小
```c

#define SHMMIN 1			 /* min shared seg size (bytes) */

/*cat /proc/sys/kernel/shmmni*/
/*sysctl -w kernel.shmmni=4096*/
#define SHMMNI 4096			 /* max num of segs system wide */

/*cat /proc/sys/kernel/shmmax*/
/*sysctl kernel.shmmax */
/*sysctl -w kernel.shmmax = xxxx*/
#define SHMMAX (ULONG_MAX - (1UL << 24)) /* max shared seg size (bytes) */

/*cat /proc/sys/kernel/shmall*/
/*sysctl -w kernel.shmall=2097152*/
#define SHMALL (ULONG_MAX - (1UL << 24)) /* max shm system wide (pages) */
#define SHMSEG SHMMNI			 /* max shared segs per process */
```
