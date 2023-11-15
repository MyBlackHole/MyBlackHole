# iostat

iostat是I/O statistics（输入/输出统计）的缩写，iostat工具将对系统的磁盘操作活动进行监视
它的特点是汇报磁盘活动统计情况，同时也会汇报出CPU使用情况。iostat也有一个弱点，就是它不能对某个进程进行深入分析，仅对系统的整体情况进行分析

```shell
[root@localhost mysql]# iostat
Linux 3.10.0-1160.el7.x86_64 (localhost)        11/14/2023      _x86_64_        (4 CPU)

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.23    0.00    0.09    1.59    0.00   98.09

Device:            tps    kB_read/s    kB_wrtn/s    kB_read    kB_wrtn
sda               4.59        40.46       286.08    3463716   24490046
dm-0              4.85        40.27       285.90    3447363   24474610
```

- cpu属性值说明：
%user：CPU处在用户模式下的时间百分比。
%nice：CPU处在带NICE值的用户模式下的时间百分比。
%system：CPU处在系统模式下的时间百分比。
%iowait：CPU等待输入输出完成时间的百分比。
%steal：管理程序维护另一个虚拟处理器时，虚拟CPU的无意识等待时间百分比。
%idle：CPU空闲时间百分比。

- 备注：
如果%iowait的值过高，表示硬盘存在I/O瓶颈
如果%idle值高，表示CPU较空闲
如果%idle值高但系统响应慢时，可能是CPU等待分配内存，应加大内存容量。
如果%idle值持续低于cpu核数，表明CPU处理能力相对较低，系统中最需要解决的资源是CPU。

- cpu属性值说明:
tps：该设备每秒的传输次数
kB_read/s：每秒从设备（drive expressed）读取的数据量；
kB_wrtn/s：每秒向设备（drive expressed）写入的数据量；
kB_read：读取的总数据量；
kB_wrtn：写入的总数量数据量；

## install
```shell
yum install sysstat -y
```

