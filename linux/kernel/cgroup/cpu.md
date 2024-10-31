# cpu

根旧版本cgroup功能类似，针对cpu的限制仍然可以支持绑定核心、配额和权重三种方式。
只是配置方法完全不一样了。
cgoup v2针对cpu资源的使用增加了压力通知机制，以便调用方可以根据相关cpu压力作出相应反馈行为。
最值得期待的就是当cpu压力达到一定程度之后实现的自动扩容了。
不过这不属于本文章讨论的范围，具体大家可以自己畅想。

## cpuset.cpus

使用 cpuset 资源隔离方式可以帮助我们把整个cgroup的进程都限定在某些cpu核心上运行，
在numa架构下，还能帮我们绑定numa结点。

- 用来制定当前 cgroup 绑定的 cpu 编号
```shell
# cat cpuset.cpus
0-4,6,8-10
```

## cpuset.cpus.effective

显示当前cgroup真实可用的cpu列表。

## cpuset.mems

用来在numa架构的服务器上绑定node结点。比如：

## cpuset.mems.effective

显示当前cgroup真实可用的mem node列表。

## cpuset.cpus.partition

这个文件可以被设置为: root 或 member
主要功能是用来设置当前cgroup是不是作为一个独立的scheduling domain进行调度。
这个功能其实就可以理解为，在root模式下，所有分配给当前cgroup的cpu都是独占这些cpu的，而member模式则可以多个cgroup之间共享cpu。
设置为root将使当前cgroup使用的cpu从上一级cgroup的cpuset.cpus.effective列表中被拿走。
设置为root之后，如果这个cgroup有下一级的cgroup，这个cgroup也将不能再切换回member状态。
在这种模式下，上一级cgroup不可以把自己所有的cpu都分配给其下一级的cgroup，其自身至少给自己留一个cpu。

设置为 root 需要当前 cgroup 符合以下条件:
1. cpuset.cpus 中设置不为空且设置的 cpu list 中的 cpu 都是独立的。就是说这些 cpu 不会共享给其他平级 cgroup。
2. 上一级 cgroup 是 partition root 配置。
3. 当前 cgroup 的 cpuset.cpus 作为集合是上一级 cgroup 的 cpuset.cpus.effective 集合的子集。
4. 下一级cgroup中没有启用cpuset资源隔离。


## cpu.max

新版 cgroup 简化了cpu配额的配置方法。用一个文件就可以进行配置了：
文件支持2个值，格式为：$MAX $PERIOD。比如这样的设置：

```shell
[root@localhost zorro]# cat /sys/fs/cgroup/zorro/cpu.max
max 100000
[root@localhost zorro]# echo 50000 100000 > /sys/fs/cgroup/zorro/cpu.max
[root@localhost zorro]# cat !$
cat /sys/fs/cgroup/zorro/cpu.max
50000 100000
```

这个含义是，在100000所表示的时间周期内，有50000是分给本cgroup的。
也就是配置了本cgroup的cpu占用在单核上不超过50%。我们来测试一下：

```shell
[root@localhost zorro]# cat while.sh
while :
do
	:
done

[root@localhost zorro]# ./while.sh &
[1] 1829

[root@localhost zorro]# echo 1829 > /sys/fs/cgroup/zorro/cgroup.procs
[root@localhost zorro]# cat !$
cat /sys/fs/cgroup/zorro/cgroup.procs
1829
[root@localhost zorro]# top
top - 16:27:00 up  2:33,  2 users,  load average: 0.28, 0.09, 0.03
Tasks: 169 total,   2 running, 167 sleeping,   0 stopped,   0 zombie
%Cpu0  :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu1  :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu2  :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu3  : 50.0 us,  0.0 sy,  0.0 ni, 50.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :   1953.2 total,   1057.1 free,    295.5 used,    600.6 buff/cache
MiB Swap:   2088.0 total,   2088.0 free,      0.0 used.   1500.7 avail Mem

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
   1829 root      20   0  227348   4092   1360 R  52.4   0.2   0:19.24 bash
      1 root      20   0  106096  15008   9548 S   0.0   0.8   0:02.40 systemd
......
```


## cpu.weight.nice

可以通过 cpu.weight 文件来设置本 cgroup 的权重值。默认为100。取值范围为[1， 10000]。

当前可以支持使用nice值的方式设置权重。取值范围根nice值范围一样\[-20, 19\]。

## cpu.stat

是当前 cgroup 的 cpu 消耗统计。
显示的内容包括：
- usage_usec: 占用cpu总时间。
- user_usec: 用户态占用时间。
- system_usec: 内核态占用时间。
- nr_periods: 周期计数。
- nr_throttled: 周期内的限制计数。
- throttled_usec: 限制执行的时间。

## cpu.pressure

显示当前cgroup的cpu使用压力状态。详情参见：Documentation/accounting/psi.rst。
psi是内核新加入的一种负载状态检测机制，可以目前可以针对cpu、memory、io的负载状态进行检测。
通过设置，我们可以让psi在相关资源负载达到一定阈值的情况下给我们发送一个事件。
用户态可以通过对文件事件的监控，实现针对相关负载作出相关相应行为的目的。
psi的话题可以单独写一个文档，所以这里不细说了。


## 例子

- 基础操作 (root)
```shell
cd /sys/fs/cgroup/

<!--创建控制组-->
mkdir cpu_test

<!--绑定cpu-->
cd /sys/fs/cgroup/cpu_test/
cat /sys/fs/cgroup/cpu_test/cgroup.procs
echo 1001 > /sys/fs/cgroup/cpu_test/cgroup.procs

<!--删除控制组-->
sudo rmdir /sys/fs/cgroup/cpu_test/
```


- 限制cpu使用率 (root)
```shell
cd /sys/fs/cgroup/
<!--创建控制组-->
mkdir cpu_test

<!--运行程序-->
test.c
#include <stdlib.h>
#include <stdio.h>
#include <stdbool.h>
#include <unistd.h>

int main()
{
	pid_t pid;
	pid = getpid();
	printf("Process ID: %d\n", pid);
	while (true) {
	}

	return EXIT_SUCCESS;
}

xmake run grammar while
Process ID: 29310

<!--绑定cpu-->
echo 29310 > /sys/fs/cgroup/cpu_test/cgroup.procs

cat /sys/fs/cgroup/cpu_test/cgroup.procs
29310

top
    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
  29310 black     20   0    1176    680    680 R 100.0   0.0   1:56.81 grammar

<!--限制cpu使用率(20%)-->
echo 2000 10000 > /sys/fs/cgroup/cpu_test/cpu.max

top
    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
  29310 black     20   0    1176    680    680 R  19.9   0.0   2:55.21 grammar
```
