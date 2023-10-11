# couting

```shell
# CPU counter statistics for the specified command:
perf stat command
 
# CPU counter statistics for the specified PID, until Ctrl-C:
perf stat -p PID
 
# CPU counter statistics for the entire system, for 5 seconds:
perf stat -a sleep 5
 
# Various basic CPU statistics, system wide, for 10 seconds:
perf stat -e cycles,instructions,cache-references,cache-misses,bus-cycles -a sleep 10
 
# Various CPU level 1 data cache statistics for the specified command:
perf stat -e L1-dcache-loads,L1-dcache-load-misses,L1-dcache-stores command
 
# Various CPU data TLB statistics for the specified command:
perf stat -e dTLB-loads,dTLB-load-misses,dTLB-prefetch-misses command
 
# Various CPU last level cache statistics for the specified command:
perf stat -e LLC-loads,LLC-load-misses,LLC-stores,LLC-prefetches command
 
# Count syscalls per-second system-wide:
perf stat -e raw_syscalls:sys_enter -I 1000 -a
 
# Count system calls by type for the specified PID, until Ctrl-C:
perf stat -e 'syscalls:sys_enter_*' -p PID
 
# Count system calls by type for the entire system, for 5 seconds:
perf stat -e 'syscalls:sys_enter_*' -a sleep 5
 
# Count scheduler events for the specified PID, until Ctrl-C:
perf stat -e 'sched:*' -p PID
 
# Count scheduler events for the specified PID, for 10 seconds:
perf stat -e 'sched:*' -p PID sleep 10
 
# Count ext4 events for the entire system, for 10 seconds:
perf stat -e 'ext4:*' -a sleep 10
 
# Count block device I/O events for the entire system, for 10 seconds:
perf stat -e 'block:*' -a sleep 10
```
