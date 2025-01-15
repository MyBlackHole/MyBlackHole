# kprobe 
用户态接口

使用前确定内核CONFIG打开:
CONFIG_KPROBE_EVENT=y
/sys/kernel/debug/tracing/kprobe_events：添加断点接口
/sys/kernel/debug/tracing/events/kprobes//enabled：断点使能开关
/sys/kernel/debug/tracing/trace：查看trace日志接口

- 删除注册的 kprobe
```shell
echo 0 > /sys/kernel/debug/tracing/events/kprobes/myprobe/enable
echo 0 > /sys/kernel/debug/tracing/events/kprobes/myretprobe/enable
echo '-:myprobe' > /sys/kernel/debug/tracing/events/kprobe_events
echo '-:myretprobe' > /sys/kernel/debug/tracing/events/kprobe_events
```

## 例子

- do_sys_open添加kprobe为例
```shell
添加kprobe:
echo 'p:myprobe do_sys_open dfd=%ax filename=%dx flags=%cx mode=+4($stack)' > /sys/kernel/debug/tracing/kprobe_events

添加kretprobe，返回值是数字：
echo 'r:myretprobe do_sys_open $retval' > /sys/kernel/debug/tracing/kprobe_events

添加kretprobe，返回值是字符串
echo 'r:myprobe getname +0($retval):string' > /sys/kernel/debug/tracing/kprobe_events

删除添加的kprobe：
echo '-:myprobe' > /sys/kernel/debug/tracing/events/kprobe_events

# echo 'p:myprobe do_sys_open dfd=%ax filename=%dx flags=%cx mode=+4($stack)' > /sys/kernel/debug/tracing/kprobe_events
# echo 'r:myretprobe do_sys_open $retval' >> /sys/kernel/debug/tracing/kprobe_events
# echo 1 > /sys/kernel/debug/tracing/events/kprobes/myprobe/enable 
# echo 1 > /sys/kernel/debug/tracing/events/kprobes/myretprobe/enable
# cat /sys/kernel/debug/tracing/trace

# tracer: nop
#
# entries-in-buffer/entries-written: 24067/24067   #P:4
#
#                              _-----=> irqs-off
#                             / _----=> need-resched
#                            | / _---=> hardirq/softirq
#                            || / _--=> preempt-depth
#                            ||| /     delay
#           TASK-PID   CPU#  ||||    TIMESTAMP  FUNCTION
#              | |       |   ||||       |         |
           <...>-54149 [002] d... 43812230.923263: myprobe: (do_sys_open+0x108/0x2a0 <- getname) arg1="^\°î¥"
   zabbix_agentd-1762  [002] d... 43812230.892155: myprobe: (do_sys_open+0x108/0x2a0 <- getname) arg1="^\°î¥"
           <...>-54040 [000] d... 43812230.896603: myprobe: (do_sys_open+0x108/0x2a0 <- getname) arg1="^\"
           <...>-54149 [003] d... 43812230.897422: myprobe: (do_sys_open+0x108/0x2a0 <- getname) arg1="^\ <81>Ë"
           <...>-54149 [003] d... 43812230.897465: myprobe: (SyS_execve+0x1b/0x50 <- getname) arg1="^\ <81>Ë"
           <...>-54149 [003] d... 43812230.897977: myprobe: (do_sys_open+0x108/0x2a0 <- getname) arg1="^\ <81>Ë"
           <...>-54149 [003] d... 43812230.897993: myprobe: (do_sys_open+0x108/0x2a0 <- getname) arg1="^\ <81>Ë"
           <...>-54149 [003] d... 43812230.898015: myprobe: (do_sys_open+0x108/0x2a0 <- getname) arg1="^\ <81>Ë"
           <...>-54149 [003] d... 43812230.898061: myprobe: (do_sys_open+0x108/0x2a0 <- getname) arg1="^\ <81>Ë"
           <...>-54149 [003] d... 43812230.898098: myprobe: (do_sys_open+0x108/0x2a0 <- getname) arg1="^\ <81>Ë"
           <...>-54149 [003] d... 43812230.898147: myprobe: (do_sys_open+0x108/0x2a0 <- getname) arg1="^\ <81>Ë
```
