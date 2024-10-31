# io

io 资源隔离相比 cgroup v1 的改进亮点就是实现了 buffer io 的限制,
让io限速使用在生产环境的条件真正成熟了。


- 实现 / 分区设置了一个 2m/s 的写入限速
```shell
[root@localhost zorro]# df
Filesystem                              1K-blocks     Used Available Use% Mounted on
devtmpfs                                   980892        0    980892   0% /dev
tmpfs                                     1000056        0   1000056   0% /dev/shm
tmpfs                                     1000056     1296    998760   1% /run
/dev/mapper/fedora_localhost--live-root  66715048 28671356  34611656  46% /
tmpfs                                     1000056        4   1000052   1% /tmp
/dev/mapper/fedora_localhost--live-home  32699156  2726884  28288204   9% /home
/dev/sda1                                  999320   260444    670064  28% /boot
tmpfs                                      200008        0    200008   0% /run/user/1000

[root@localhost zorro]# ls -l /dev/mapper/fedora_localhost--live-root
lrwxrwxrwx. 1 root root 7 Apr 14 13:53 /dev/mapper/fedora_localhost--live-root -> ../dm-0

[root@localhost zorro]# ls -l /dev/dm-0
brw-rw----. 1 root disk 253, 0 Apr 14 13:53 /dev/dm-0

[root@localhost zorro]# echo "253:0 wbps=2097152" > /sys/fs/cgroup/zorro/io.max

[root@localhost zorro]# cat !$
cat /sys/fs/cgroup/zorro/io.max
253:0 rbps=max wbps=2097152 riops=max wiops=max

[root@localhost zorro]# cat dd.sh
#!/bin/bash

echo $$ > /sys/fs/cgroup/zorro/cgroup.procs
dd if=/dev/zero of=/bigfile bs=1M count=200

<!--我们会发现，这时dd很快就把数据写到了缓存里。这里要看到限速效果，需要同时通过iostat监控针对块设备的写入-->
[root@localhost zorro]# ./dd.sh
200+0 records in
200+0 records out
209715200 bytes (210 MB, 200 MiB) copied, 0.208817 s, 1.0 GB/s

<!--命令执行期间，我们会发现iostat中，针对设备的 write 被限制在了2m/s-->

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.00    0.00    0.25    2.24    0.00   97.51

Device             tps    kB_read/s    kB_wrtn/s    kB_dscd/s    kB_read    kB_wrtn    kB_dscd
dm-0             22.00         0.00      2172.00         0.00          0       2172          0
dm-1              0.00         0.00         0.00         0.00          0          0          0
dm-2              0.00         0.00         0.00         0.00          0          0          0
sda              15.00         0.00      2172.00         0.00          0       2172          0
sdb               0.00         0.00         0.00         0.00          0          0          0
scd0              0.00         0.00         0.00         0.00          0          0          0


avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.00    0.00    0.00   23.44    0.00   76.56

Device             tps    kB_read/s    kB_wrtn/s    kB_dscd/s    kB_read    kB_wrtn    kB_dscd
dm-0             14.00         0.00      2080.00         0.00          0       2080          0
dm-1              0.00         0.00         0.00         0.00          0          0          0
dm-2              0.00         0.00         0.00         0.00          0          0          0
sda              14.00         0.00      2080.00         0.00          0       2080          0
sdb               0.00         0.00         0.00         0.00          0          0          0
scd0              0.00         0.00         0.00         0.00          0          0          0


avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.00    0.00    0.25   21.70    0.00   78.05

Device             tps    kB_read/s    kB_wrtn/s    kB_dscd/s    kB_read    kB_wrtn    kB_dscd
dm-0             14.00         0.00      2052.00         0.00          0       2052          0
dm-1              0.00         0.00         0.00         0.00          0          0          0
dm-2              0.00         0.00         0.00         0.00          0          0          0
sda              14.00         0.00      2052.00         0.00          0       2052          0
sdb               0.00         0.00         0.00         0.00          0          0          0
scd0              0.00         0.00         0.00         0.00          0          0          0


<!--标记了 direct 的 io 事件限速效果根之前一样-->

[root@localhost zorro]# cat dd.sh
#!/bin/bash

echo $$ > /sys/fs/cgroup/zorro/cgroup.procs
dd if=/dev/zero of=/bigfile bs=1M count=200 oflag=direct

[root@localhost zorro]# ./dd.sh
200+0 records in
200+0 records out
209715200 bytes (210 MB, 200 MiB) copied, 100.007 s, 2.1 MB/s
```

## io.max

我们刚才已经使用了这个文件进行了写速率限制，wbps。
除此以外，还支持:
rbps: 读速率限制。
riops: 读iops限制。
wiops：写iops限制。

- 在一条命令中可以写多个限制，比如：

```shell
echo “8:16 rbps=2097152 wiops=120” > io.max
```

## io.stat

查看本cgroup的io相关信息统计。包括：

```shell
======        =====================
rbytes        Bytes read
wbytes        Bytes written
rios          Number of read IOs
wios          Number of write IOs
dbytes        Bytes discarded
dios          Number of discard IOs
======        =====================
```

## io.weight

权重方式分配io资源的接口。
默认为：default 100。
default 可以替换成 $MAJ:$MIN 表示的设备编号，如: "8:0" 表示针对那个设备的配置。
后面的100表示权重，取值范围是：[1, 10000]。
表示本cgroup中的进程使用某个设备的io权重是多少？
如果有多个cgroup同时争抢一个设备的io使用的话，他们将按权重进行io资源分配。

## io.bfq.weight

针对bfq的权重配置文件。

## io.latency

这是cgroup v2实现的一种对io负载保护的机制。

- 可以给一个磁盘设置一个预期延时目标
```shell
[root@localhost zorro]# echo "253:0 target=100" > /sys/fs/cgroup/zorro/io.latency

[root@localhost zorro]# cat !$
cat /sys/fs/cgroup/zorro/io.latency
253:0 target=100

<!--target的单位是ms。-->
<!--如果cgroup检测到当前cgroup内的io响应延迟时间超过了这个target，-->
<!--那么cgroup可能会限制同一个父级cgroup下的其他同级别cgroup的io负载，-->
<!--以尽量让当前cgroup的target达到预期。-->
<!--更详细文档可以查看：Documentation/admin-guide/cgroup-v2.rst-->
```

## io.pressure

当前cgroup的io资源的psi接口文件。
