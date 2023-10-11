# perf

## 安装
```shell
sudo apt install linux-tools-common
```

|   |   |   |
|---|---|---|
|序号|命令|作用|
|1|annotate|解析perf record生成的perf.data文件，显示被注释的代码。|
|2|archive|根据数据文件记录的build-id，将所有被采样到的elf文件打包。利用此压缩包，可以再任何机器上分析数据文件中记录的采样数据。|
|3|bench|perf中内置的benchmark，目前包括两套针对调度器和内存管理子系统的benchmark。|
|4|buildid-cache|管理perf的buildid缓存，每个elf文件都有一个独一无二的buildid。buildid被perf用来关联性能数据与elf文件。|
|5|buildid-list|列出数据文件中记录的所有buildid。|
|6|diff|对比两个数据文件的差异。能够给出每个符号（函数）在热点分析上的具体差异。|
|7|evlist|列出数据文件perf.data中所有性能事件。|
|8|inject|该工具读取perf record工具记录的事件流，并将其定向到标准输出。在被分析代码中的任何一点，都可以向事件流中注入其它事件。|
|9|kmem|针对内核内存（slab）子系统进行追踪测量的工具|
|10|kvm|用来追踪测试运行在KVM虚拟机上的Guest OS。|
|11|list|列出当前系统支持的所有性能事件。包括硬件性能事件、软件性能事件以及检查点。|
|12|lock|分析内核中的锁信息，包括锁的争用情况，等待延迟等。|
|13|mem|内存存取情况|
|14|record|收集采样信息，并将其记录在数据文件中。随后可通过其它工具对数据文件进行分析。|
|15|report|读取perf record创建的数据文件，并给出热点分析结果。|
|16|sched|针对调度器子系统的分析工具。|
|17|script|执行perl或python写的功能扩展脚本、生成脚本框架、读取数据文件中的数据信息等。|
|18|stat|执行某个命令，收集特定进程的性能概况，包括CPI、Cache丢失率等。|
|19|test|perf对当前软硬件平台进行健全性测试，可用此工具测试当前的软硬件平台是否能支持perf的所有功能。|
|20|timechart|针对测试期间系统行为进行可视化的工具|
|21|top|类似于linux的top命令，对系统性能进行实时分析。|
|22|trace|关于syscall的工具。|
|23|probe|用于定义动态检查点。|

## list

Perf List：查看当前软硬件平台支持的性能事件列表，性能事件的属性。

1. u: 仅统计用户空间程序触发的性能事件
2. k: 仅统计内核触发的性能事件
3. h: 仅统计 Hypervisor 触发的性能事件
4. G: 在 KVM 虚拟机中，仅统计 Guest 系统触发的性能事件
5. H: 仅统计 Host 系统触发的性能事件
6. p: 精度级别


## stat

Perf Stat：分析性能。
```shell
perf stat -p $pid -d     # 进程级别统计
perf stat -a -d sleep 5  # 系统整体统计
perf stat -p $pid -e 'syscalls:sys_enter' sleep 10  #分析进程调用系统调用的情形
```

## top

Perf Top：实时显示系统/进程的性能统计信息, 默认性能事件为 cycles ( CPU 周期数 )。与 Linux top tool 功能类似。
```shell
perf top -p $pid -g     # 进程级别
perf top -g  # 系统整体
```

## record

Perf Record：记录一段时间内系统/进程的性能事件, 默认性能事件为 cycles ( CPU 周期数 )。

- 采集数据
```shell
perf record -e cpu-clock -g -p 44160

perf record -p $pid -g -e cycles -e cs #进程采样
perf record -a -g -e cycles -e cs #系统整体采样
```

## script

读取 Perf Record 结果。

```shell
-i, --input <file>    input file name
-G, --hide-call-graph
                      When printing symbols do not display call chain
-F, --fields <str>    comma separated output fields prepend with 'type:'. Valid types: hw,sw,trace,raw. Fields: comm,tid,pid,time,cpu,event,trace,ip,sym,dso,addr,symoff,period
-a, --all-cpus        system-wide collection from all CPUs
-S, --symbols <symbol[,symbol...]>
                      only consider these symbols
-C, --cpu <cpu>       list of cpus to profile
-c, --comms <comm[,comm...]>
                      only display events for these comms
    --pid <pid[,pid...]>
                      only consider symbols in these pids
    --tid <tid[,tid...]>
                      only consider symbols in these tids
    --time <str>      Time span of interest (start,stop)
    --show-kernel-path
                      Show the path of [kernel.kallsyms]
    --show-task-events
                      Show the fork/comm/exit events
    --show-mmap-events
                      Show the mmap events
    --per-event-dump  Dump trace output to files named by the monitored events
```

## report

- 浏览数据
```shell
perf report
```
