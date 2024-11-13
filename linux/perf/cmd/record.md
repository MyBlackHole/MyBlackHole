# record

Perf Record：记录一段时间内系统/进程的性能事件, 默认性能事件为 cycles ( CPU 周期数 )。

- 记录指定进程的性能数据
```shell
perf record -p $pid -g
```


- 采集数据
```shell
perf record -e cpu-clock -g -p 44160

perf record -p $pid -g -e cycles -e cs #进程采样
perf record -a -g -e cycles -e cs #系统整体采样
```


```shell
# Sample on-CPU functions for the specified command, at 99 Hertz:
perf record -F 99 command
 
# Sample on-CPU functions for the specified PID, at 99 Hertz, until Ctrl-C:
perf record -F 99 -p PID
 
# Sample on-CPU functions for the specified PID, at 99 Hertz, for 10 seconds:
perf record -F 99 -p PID sleep 10
 
# Sample CPU stack traces (via frame pointers) for the specified PID, at 99 Hertz, for 10 seconds:
perf record -F 99 -p PID -g -- sleep 10
 
# Sample CPU stack traces for the entire system, at 99 Hertz, for 10 seconds (< Linux 4.11):
perf record -F 99 -ag -- sleep 10
 
# Sample CPU stack traces for the entire system, at 99 Hertz, for 10 seconds (>= Linux 4.11):
perf record -F 99 -g -- sleep 10
 
# If the previous command didn't work, try forcing perf to use the cpu-clock event:
perf record -F 99 -e cpu-clock -ag -- sleep 10
 
# Sample CPU stack traces, once every 10,000 Level 1 data cache misses, for 5 seconds:
perf record -e L1-dcache-load-misses -c 10000 -ag -- sleep 5
 
# Sample CPU stack traces, once every 100 last level cache misses, for 5 seconds:
perf record -e LLC-load-misses -c 100 -ag -- sleep 5 
 
# Sample on-CPU kernel instructions, for 5 seconds:
perf record -e cycles:k -a -- sleep 5 
 
# Sample on-CPU user instructions, for 5 seconds:
perf record -e cycles:u -a -- sleep 5 
```

```shell
# Trace new processes, until Ctrl-C:
perf record -e sched:sched_process_exec -a
 
# Sample context-switches, until Ctrl-C:
perf record -e context-switches -a
 
# Trace all context-switches, until Ctrl-C:
perf record -e context-switches -c 1 -a
 
# Trace all context-switches via sched tracepoint, until Ctrl-C:
perf record -e sched:sched_switch -a
 
# Sample context-switches with stack traces, until Ctrl-C:
perf record -e context-switches -ag
 
# Sample context-switches with stack traces, for 10 seconds:
perf record -e context-switches -ag -- sleep 10
 
# Sample CS, stack traces, and with timestamps (< Linux 3.17, -T now default):
perf record -e context-switches -ag -T
 
# Sample CPU migrations, for 10 seconds:
perf record -e migrations -a -- sleep 10
 
# Trace all connect()s with stack traces (outbound connections), until Ctrl-C:
perf record -e syscalls:sys_enter_connect -ag
 
# Trace all accepts()s with stack traces (inbound connections), until Ctrl-C:
perf record -e syscalls:sys_enter_accept* -ag
 
# Trace all block device (disk I/O) requests with stack traces, until Ctrl-C:
perf record -e block:block_rq_insert -ag
 
# Trace all block device issues and completions (has timestamps), until Ctrl-C:
perf record -e block:block_rq_issue -e block:block_rq_complete -a
 
# Trace all block completions, of size at least 100 Kbytes, until Ctrl-C:
perf record -e block:block_rq_complete --filter 'nr_sector > 200'
 
# Trace all block completions, synchronous writes only, until Ctrl-C:
perf record -e block:block_rq_complete --filter 'rwbs == "WS"'
 
# Trace all block completions, all types of writes, until Ctrl-C:
perf record -e block:block_rq_complete --filter 'rwbs ~ "*W*"'
 
# Sample minor faults (RSS growth) with stack traces, until Ctrl-C:
perf record -e minor-faults -ag
 
# Trace all minor faults with stack traces, until Ctrl-C:
perf record -e minor-faults -c 1 -ag
 
# Sample page faults with stack traces, until Ctrl-C:
perf record -e page-faults -ag
 
# Trace all ext4 calls, and write to a non-ext4 location, until Ctrl-C:
perf record -e 'ext4:*' -o /tmp/perf.data -a 
 
# Trace kswapd wakeup events, until Ctrl-C:
perf record -e vmscan:mm_vmscan_wakeup_kswapd -ag
 
# Add Node.js USDT probes (Linux 4.10+):
perf buildid-cache --add `which node`
 
# Trace the node http__server__request USDT event (Linux 4.10+):
perf record -e sdt_node:http__server__request -a
```
