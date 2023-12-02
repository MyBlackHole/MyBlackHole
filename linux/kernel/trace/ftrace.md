# ftrace

内核跟踪工具

1. 跟踪点(tracepoint)：一种基于静态测试代码的工作机制
2. 探针(kprobe)：一种动态跟踪机制，用于在任意时刻中断内核代码的运行，调用它自己的处理程序，在完成需要的操作之后再返回
3. perf_events ：一个访问 PMU（性能监视单元Performance Monitoring Unit）的接口


## config
ftrace 工作在 debugfs 文件系统上
/sys/kernel/debug/tracing
```shell
root@black:/sys/kernel/debug/tracing# ls
available_events            dyn_ftrace_total_info     kprobe_events    rv                     set_ftrace_notrace      stack_trace_filter  trace_pipe
available_filter_functions  enabled_functions         kprobe_profile   saved_cmdlines         set_ftrace_notrace_pid  synthetic_events    trace_stat
available_tracers           error_log                 max_graph_depth  saved_cmdlines_size    set_ftrace_pid          timestamp_mode      tracing_cpumask
buffer_percent              events                    options          saved_tgids            set_graph_function      trace               tracing_max_latency
buffer_size_kb              free_buffer               osnoise          set_event              set_graph_notrace       trace_clock         tracing_on
buffer_total_size_kb        function_profile_enabled  per_cpu          set_event_notrace_pid  snapshot                trace_marker        tracing_thresh
current_tracer              hwlat_detector            printk_formats   set_event_pid          stack_max_size          trace_marker_raw    uprobe_events
dynamic_events              instances                 README           set_ftrace_filter      stack_trace             trace_options       uprobe_profile
```

1. available_tracers —— 可用的跟踪程序
2. current_tracer —— 正在运行的跟踪程序
3. tracing_on —— 负责启用或禁用数据写入到 Ring 缓冲区的系统文件（如果启用它，数字 1 被添加到文件中，禁用它，数字 0 被添加）
4. trace —— 以人类友好格式保存跟踪数据的文件

## 可用跟踪

```shell
root@black:/sys/kernel/debug/tracing# cat available_tracers
timerlat osnoise hwlat blk mmiotrace function_graph wakeup_dl wakeup_rt wakeup function nop
```
1. function —— 一个无需参数的函数调用跟踪程序
2. function_graph —— 一个使用子调用的函数调用跟踪程序
3. blk —— 一个与块 I/O 跟踪相关的调用和事件跟踪程序（它是 blktrace 使用的）
4. mmiotrace —— 一个内存映射 I/O 操作跟踪程序
5. nop —— 最简单的跟踪程序，就像它的名字所暗示的那样，它不做任何事情（尽管在某些情况下可能会派上用场，我们将在后文中详细解释）

### 函数跟踪程序

- test
```shell
#!/bin/sh
dir=/sys/kernel/debug/tracing
# 启用函数跟踪
sysctl kernel.ftrace_enabled=1
echo function > ${dir}/current_tracer
echo 1 > ${dir}/tracing_on
sleep 1
echo 0 > ${dir}/tracing_on
less ${dir}/trace


# tracer: function
#
# entries-in-buffer/entries-written: 820000/9674135   #P:16
#
#                                _-----=> irqs-off/BH-disabled
#                               / _----=> need-resched
#                              | / _---=> hardirq/softirq
#                              || / _--=> preempt-depth
#                              ||| / _-=> migrate-disable
#                              |||| /     delay
#           TASK-PID     CPU#  |||||  TIMESTAMP  FUNCTION
#              | |         |   |||||     |         |
          <idle>-0       [012] d..2. 449236.934595: leave_mm <-cpuidle_enter_state
          <idle>-0       [012] d..2. 449236.934596: switch_mm <-leave_mm
          <idle>-0       [012] d..2. 449236.934596: switch_mm_irqs_off <-switch_mm
          <idle>-0       [012] d..2. 449236.934597: load_new_mm_cr3 <-switch_mm_irqs_off
          <idle>-0       [012] d..2. 449236.934598: switch_ldt <-switch_mm_irqs_off
          <idle>-0       [012] d..2. 449236.934598: sched_idle_set_state <-cpuidle_enter_state
          <idle>-0       [012] d..2. 449236.934599: acpi_idle_enter <-cpuidle_enter_state
          <idle>-0       [012] d..2. 449236.934600: acpi_idle_enter_bm <-acpi_idle_enter
          <idle>-0       [012] d..2. 449236.934600: acpi_read_bit_register <-acpi_idle_enter_bm
          <idle>-0       [012] d..2. 449236.934601: acpi_ut_trace_u32 <-acpi_read_bit_register
          <idle>-0       [012] d..2. 449236.934602: acpi_hw_get_bit_register_info <-acpi_read_bit_register
          <idle>-0       [012] d..2. 449236.934603: acpi_ut_track_stack_ptr <-acpi_hw_get_bit_register_info
          <idle>-0       [012] d..2. 449236.934603: acpi_hw_register_read <-acpi_read_bit_register
          <idle>-0       [012] d..2. 449236.934604: acpi_ut_trace <-acpi_hw_register_read
          <idle>-0       [012] d..2. 449236.934604: acpi_hw_read <-acpi_hw_register_read
          <idle>-0       [012] d..2. 449236.934605: acpi_hw_validate_register <-acpi_hw_read
          <idle>-0       [012] d..2. 449236.934605: acpi_hw_get_access_bit_width <-acpi_hw_validate_register
          <idle>-0       [012] d..2. 449236.934606: acpi_hw_get_access_bit_width <-acpi_hw_read
          <idle>-0       [012] d..2. 449236.934606: acpi_hw_read_port <-acpi_hw_read
          <idle>-0       [012] d..2. 449236.934607: acpi_hw_validate_io_request <-acpi_hw_read_port
          <idle>-0       [012] d..2. 449236.934607: acpi_ut_trace <-acpi_hw_validate_io_request
          <idle>-0       [012] d..2. 449236.934608: acpi_ut_status_exit <-acpi_hw_validate_io_request
          <idle>-0       [012] d..2. 449236.934608: acpi_os_read_port <-acpi_hw_read_port
          <idle>-0       [012] d..2. 449236.934610: acpi_ut_status_exit <-acpi_hw_register_read
          <idle>-0       [012] d..2. 449236.934611: acpi_ut_status_exit <-acpi_read_bit_register


进程标识符（PID）
运行这个进程的 CPU（CPU#）
进程开始时间（TIMESTAMP）
被跟踪函数的名字以及调用它的父级函数；例如，在我们输出的第一行，rb_simple_write 调用了 mutex-unlock 函数。
```


### function_graph

- test
```shell
#!/bin/sh
dir=/sys/kernel/debug/tracing
sysctl kernel.ftrace_enabled=1
echo function_graph > ${dir}/current_tracer
echo 1 > ${dir}/tracing_on
sleep 1
echo 0 > ${dir}/tracing_on
less ${dir}/trace

# tracer: function_graph
#
# CPU  DURATION                  FUNCTION CALLS
# |     |   |                     |   |   |   |
  9)   0.662 us    |          } /* sched_idle_set_state */
  9)               |          acpi_idle_enter() {
  9)               |            acpi_idle_do_entry() {
  9) ! 855.106 us  |              acpi_processor_ffh_cstate_enter();
  9) ! 856.750 us  |            }
  9) ! 858.754 us  |          }
  9)               |          ktime_get() {
  9)   1.623 us    |            read_hpet();
  9)   3.908 us    |          }
  9)   1.183 us    |          sched_idle_set_state();
  9)               |          irq_enter_rcu() {
  9)               |            tick_irq_enter() {
  9)   1.072 us    |              tick_check_oneshot_broadcast_this_cpu();
  9)               |              ktime_get() {
  9)   1.212 us    |                read_hpet();
  9)   3.256 us    |              }
  9)   1.142 us    |              nr_iowait_cpu();
  9)               |              ktime_get() {
  9)   1.583 us    |                read_hpet();
  9)   3.898 us    |              }
  9) + 14.358 us   |            }
  9) + 16.843 us   |          }
  9)               |          __sysvec_apic_timer_interrupt() {
  9)               |            hrtimer_interrupt() {
  9)   1.262 us    |              _raw_spin_lock_irqsave();
  9)               |              ktime_get_update_offsets_now() {
  9)   1.814 us    |                read_hpet();
  9)   8.918 us    |              }
  9)               |              __hrtimer_run_queues() {
  9)   1.373 us    |                __remove_hrtimer();
  9)   1.012 us    |                _raw_spin_unlock_irqrestore();
  9)               |                tick_sched_timer() {
  9)               |                  ktime_get() {


DURATION 展示了花费在每个运行的函数上的时间。注意使用 + 和 ! 符号标记的地方。加号（+）意思是这个函数花费的时间超过 10 毫秒；而感叹号（!）意思是这个函数花费的时间超过了 100 毫秒。
在 FUNCTION_CALLS 下面，我们可以看到每个函数调用的信息。
和 C 语言一样使用了花括号（{）标记每个函数的边界，它展示了每个函数的开始和结束，一个用于开始，一个用于结束；不能调用其它任何函数的叶子函数用一个分号（;）标记。
```


## 函数过滤器
ftrace 输出可能会很大，精确找出你所需要的内容可能会非常困难。我们可以使用过滤器去简化我们的搜索：
输出中将只显示与我们感兴趣的函数相关的信息。

- 为实现过滤，我们只需要在 set_ftrace_filter 文件中写入我们需要过滤的函数的名字即可
```shelll
root@andrei:/sys/kernel/debug/tracing# echo kfree > set_ftrace_filter
```

- 禁用过滤器，我们只需要在这个文件中添加一个空白行即可
```shell
root@andrei:/sys/kernel/debug/tracing# echo  > set_ftrace_filter
```

- 通过运行这个命令: 我们将得到相反的结果：输出将包含除了 kfree() 以外的任何函数的信息。
```shell
root@andrei:/sys/kernel/debug/tracing# echo kfree > set_ftrace_notrace 
```

另一个有用的选项是 set_ftrace_pid。它是为在一个特定的进程运行期间调用跟踪函数准备的。


## 跟踪事件

我们在上面提到到跟踪点机制。跟踪点是插入的触发系统事件的特定代码。跟踪点可以是动态的（意味着可能会在它们上面附加几个检查），也可以是静态的（意味着不会附加任何检查）。
静态跟踪点不会对系统有任何影响；它们只是在测试的函数末尾增加几个字节的函数调用以及在一个独立的节上增加一个数据结构。
当相关代码片断运行时，动态跟踪点调用一个跟踪函数。跟踪数据是写入到 Ring 缓冲区。
跟踪点可以设置在代码的任何位置；事实上，它们确实可以在许多的内核函数中找到。我们来看一下 kmem_cache_alloc 函数（取自 这里）：

```c
{
    void *ret = slab_alloc(cachep, flags, _RET_IP_);
    trace_kmem_cache_alloc(_RET_IP_, ret,cachep->object_size, cachep->size, flags);
    return ret;
}

// trace_kmem_cache_alloc 它本身就是一个跟踪点。我们可以通过查看其它内核函数的源代码找到这样无数的例子
```

- 事件列表
```shell
root@andrei:/sys/kernel/debug/tracing# cat available_events

root@black:/sys/kernel/debug/tracing# ls events/
alarmtimer    cpuhp         filemap         initcall      iwlwifi_ucode  mmap       page_isolation  rpm      sync_trace               vmalloc
amd_cpu       cros_ec       fs_dax          intel_iommu   jbd2           mmap_lock  pagemap         rseq     syscalls                 vmscan
amdgpu        dev           ftrace          interconnect  kmem           mmc        page_pool       rtc      target                   vsyscall
amdgpu_dm     devfreq       gpio            iocost        kvm            module     percpu          rv       task                     watchdog
asoc          devlink       gpu_scheduler   iomap         kvmmmu         mptcp      power           sched    tcp                      wbt
avc           dma_fence     hda             iommu         libata         msr        printk          scsi     thermal                  workqueue
block         drm           hda_controller  io_uring      lock           napi       pwm             sd       thermal_power_allocator  writeback
bpf_test_run  enable        hda_intel       irq           mac80211       neigh      qdisc           signal   thp                      x86_fpu
bpf_trace     error_report  header_event    irq_matrix    mac80211_msg   net        ras             skb      timer                    xdp
bridge        exceptions    header_page     irq_vectors   maple_tree     netlink    raw_syscalls    smbus    tlb                      xen
cfg80211      ext4          huge_memory     iwlwifi       mce            nmi        rcu             sock     ucsi                     xhci-hcd
cgroup        fib           hwmon           iwlwifi_data  mctp           nvme       regmap          sof      udp
clk           fib6          hyperv          iwlwifi_io    mdio           oom        regulator       spi      v4l2
compaction    filelock      i2c             iwlwifi_msg   migrate        osnoise    resctrl         swiotlb  vb2
```

```shell
root@andrei:/sys/kernel/debug/tracing# cat tracing_on
<!-- 如果在控制台中显示的是数字 0，那么，我们可以运行如下的命令来启用它 -->
root@andrei:/sys/kernel/debug/tracing# echo 1 > tracing_on

我们来跟踪访问一下chroot系统调用。对于我们的跟踪程序，我们使用 nop 因为函数跟踪程序和 function_graph 跟踪程序记录的信息太多，它包含了我们不感兴趣的事件信息。
root@andrei:/sys/kernel/debug/tracing# echo nop > current_tracer
root@andrei:/sys/kernel/debug/tracing# echo 1 > events/syscalls/sys_enter_chroot/enable

root@andrei:/sys/kernel/debug/tracing# echo 0 > tracing_on

cat trace
# tracer: nop
#
# entries-in-buffer/entries-written: 2/2   #P:16
#
#                                _-----=> irqs-off/BH-disabled
#                               / _----=> need-resched
#                              | / _---=> hardirq/softirq
#                              || / _--=> preempt-depth
#                              ||| / _-=> migrate-disable
#                              |||| /     delay
#           TASK-PID     CPU#  |||||  TIMESTAMP  FUNCTION
#              | |         |   |||||     |         |
              sh-3442906 [002] ..... 450108.473917: sys_chroot(filename: 5570e93630ed)
           a.out-3445425 [006] ..... 450134.333248: sys_chroot(filename: 55ad1cbc60ed)
```
