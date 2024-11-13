# top

Perf Top:
实时显示系统/进程的性能统计信息, 默认性能事件为 cycles ( CPU 周期数 )。
与 Linux top tool 功能类似。

```shell
perf top -p $pid -g     # 进程级别
perf top -g  # 系统整体
```

