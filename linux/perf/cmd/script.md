# script

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

