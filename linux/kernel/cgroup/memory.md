# memory

我们最常用也最好理解的就是对内存使用限制一个上限，应用程序使用不能超过此上限，超过就会oom，这就是硬限制。

## memory.max

默认值为max，不限制。如果需要做限制，则写一个内存字节数上限到文件内就可以了。

## memory.swap.max

使用swap的上限，默认为max。如果不想使用swap，设置此值为0。

## memory.min

这是内存的硬保护机制。如果当前cgroup的内存使用量在min值以内，则任何情况下都不会对这部分内存进行回收。
如果没有可用的不受保护的可回收内存，则将oom。
这个值会受到上层cgroup的min限制影响，如果所有子一级的min限制总数大于上一级cgroup的min限制，
当这些子一级cgroup都要使用申请内存的时候，其总量不能超过上一级cgroup的min。这种情况下，
各个cgroup的受保护内存按照min值的比率分配。
如果将min值设置的比你当前可用内存还大，可能将导致持续不断的oom。如果cgroup中没有进程，这个值将被忽略。

## memory.current

显示当前cgroup内存使用总数。当然也包括其子孙cgroup。

## memory.swap.current

显示当前cgroup的swap使用总数。

## memory.high

内存使用的上限限制。
与max不同，max会直接触发oom。而内存使用超出这个上限会让当前cgroup承受更多的内存回收压力。
内核会尽量使用各种手段回收内存，保持内存使用减少到memory.high限制以下。

## memory.low

cgroup内存使用如果低于这个值，则内存将尽量不被回收。
这是一种是尽力而为的内存保护，这是“软保证”，如果cgroup及其所有子代均低于此阈值，除非无法从任何未受保护的cgroup回收内存，否则不会回收cgroup的内存。

## memory.oom.group

默认值为0，值为1之后在内存超限发生oom的时候，会将整个cgroup内的所有进程都干掉，oom_score_adj设置为-1000的除外。

## memory.stat

类似meminfo的更详细的内存使用信息统计。

## memory.events

跟内存限制的相关事件触发次数统计，包括了所有子一级cgroup的相关统计。

## memory.events.local

跟上一个一样，但是只统计自己的（不包含其他子一级cgroup）。

## memory.swap.events

根swap限制相关的事件触发次数统计。

以上events文件在发生相关值变化的时候都会触发一个io事件，
可以使用poll或select来接收并处理这些事件，已实现各种事件的上层相应机制。

## memory.pressure

当前cgroup内存使用的psi接口文件。

## 示例

- user.slice 修改内存限制为100M (不建议使用)
```shell
systemctl show user.slice | grep MemoryLimit
MemoryLimit=infinity

systemctl status user.slice
● user.slice - User and Session Slice
     Loaded: loaded (/usr/lib/systemd/system/user.slice; static)
     Active: active since Wed 2024-10-30 11:31:59 CST; 2h 26min ago
 Invocation: c54e6d702b514467ab76ee65f07e230c
       Docs: man:systemd.special(7)
      Tasks: 1185
     Memory: 6G (peak: 9.6G)
        CPU: 53min 22.528s
     CGroup: /user.slice
             └─user-1000.slice
               ├─session-2.scope
               │ ├─  778 /usr/lib/sddm/sddm-helper --socket /tmp/sddm-auth-482c9bc6-d5e7-4e38-a7ec-d87d53c055d0 --id 1 --start Hyprland --user black
               │ ├─  814 Hyprland
               │ ├─  901 waybar
               │ ├─  902 /usr/lib/polkit-gnome/polkit-gnome-authentication-agent-1
               │ ├─  904 /usr/bin/python /usr/bin/blueman-applet
               │ ├─  906 /usr/bin/python /usr/bin/udiskie --no-automount --smart-tray
               │ ├─  913 nm-applet --indicator
               │ ├─  915 dunst
               │ ├─  917 wl-paste --type text --watch cliphist store
               │ ├─  923 wl-paste --type image --watch cliphist store

systemctl set-property user.slice MemoryLimit=100M


ls -alh /sys/fs/cgroup/user.slice/memory.max
Permissions Size User Date Modified Name
.rw-r--r--     0 root 30 Oct 13:59   /sys/fs/cgroup/user.slice/memory.max

cat /sys/fs/cgroup/user.slice/memory.max
104857600

```


- 限制进程的内存使用
```shell
cd /sys/fs/cgroup/
<!--创建控制组-->
mkdir cpu_test

#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <stdbool.h>
#include <unistd.h>

#define MB (1024 * 1024)

#define GB (1024 * 1024 * 1024)

/*
 * 此案例用来测试 cgroup v2 cpu、memory 限制功能
 */
int demo_while_main()
{
	char *p;
	int i = 0;
	pid_t pid;
	pid = getpid();
	printf("Process ID: %d\n", pid);
	while (true) {
		if (i * MB < GB) {
			p = (char *)malloc(MB);
			memset(p, 0, MB);
			printf("%dM memory allocated\n", ++i);
		}
		sleep(1);
	}

	return EXIT_SUCCESS;
}

xmake run grammar while
Process ID: 31745
1M memory allocated
2M memory allocated
3M memory allocated
4M memory allocated
5M memory allocated
6M memory allocated
7M memory allocated
8M memory allocated

<!--绑定cpu-->
echo 31745 > /sys/fs/cgroup/cpu_test/cgroup.procs

cat /sys/fs/cgroup/cpu_test/cgroup.procs
31745

cat /sys/fs/cgroup/cpu_test/memory.max
max

<!--限制内存使用大小(100M)-->
echo 104857600 > /sys/fs/cgroup/cpu_test/memory.max

98M memory allocated
99M memory allocated
100M memory allocated
101M memory allocated
102M memory allocated
103M memory allocated
104M memory allocated
105M memory allocated
106M memory allocated
107M memory allocated
error: execv(/run/media/black/Data/Documents/c/build/linux/x86_64/debug/grammar while) failed(-1)


echo 10485760 > /sys/fs/cgroup/cpu_test/memory.high

(会堵塞)
19M memory allocated
20M memory allocated
21M memory allocated
22M memory allocated
23M memory allocated
24M memory allocated
^C

<!--先退出进程('cpu_test': Device or resource busy)-->
rmdir /sys/fs/cgroup/cpu_test
```
