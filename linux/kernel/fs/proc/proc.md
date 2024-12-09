# proc

```shell
sudo mount -t proc none /proc
```


## /proc/sys/

### /proc/sys/kernel/





#### panic

此配置决定了内核触发 panic 后的行为

| value |       含义        |
| :---: | :-------------: |
|   0   |    内核持续 loop    |
|  负数   |     内核立刻重启      |
|  正数   | 内核将会在配置的 s 数后重启 |


#### panic_on_oops

配置当一个 oops、内核 BUG 产生时内核的行为。

| value |                      含义                      |
| :---: | :------------------------------------------: |
|   0   |                    尝试继续运行                    |
|   1   | 立刻触发 panic（当 sysctl panic 配置设置为非 0 值时机器将会重启） |
```
❯ cat /proc/sys/kernel/panic_on_oops
1

```


#### ftrace_dump_on_oops
配置当内核 oops 时是否调用 `ftrace_dump` ftrace 缓冲区的内容到串口中，可用于获取触发内核 oops 时的 ftrace 跟踪内容来定位问题原因。

|value|含义|
|:-:|:-:|
|0|关闭（默认配置）|
|1|dump 所有 cpu 的 ftrace buffers|
|2|dump 触发 oops cpu 的 ftrace buffers|
```
❯ cat /proc/sys/kernel/ftrace_dump_on_oops
0
```

#### hardlockup_all_cpu_backtrace

此配置控制当 hard lockup 检测器检测到一个 hard lockup 时是否收集更多调试信息的行为。当使能时，将会 dump 每种硬件架构特定的 cpu 栈信息。

| value |               含义               |
| :---: | :----------------------------: |
|   0   |            关闭（默认配置）            |
|   1   | 当 hardlockup 检测到时收集所有 cpu 的栈信息 |
```
❯ cat /proc/sys/kernel/hardlockup_all_cpu_backtrace
0
```

#### hardlockup_panic

配置在检测到 hard lockup 产生时是否触发内核 panic。

|value|含义|
|:-:|:-:|
|0|不触发 panic|
|1|触发 panic|
```
❯ cat /proc/sys/kernel/hardlockup_panic
1
```

#### hung_task_all_cpu_backtrace

当此选项设置时，当检测到一个 hung 住的任务时内核会向所有的 CPU 发送一个 NMI 中断来 dump 栈回溯信息。当 `CONFIG_DETECT_HUNG_TASK`与`CONFIG_SMP`内核配置开启时此项配置才会存在。

| value |    含义    |
| :---: | :------: |
|   0   | 关闭（默认配置） |
|   1   |    使能    |
```
❯ cat /proc/sys/kernel/hung_task_all_cpu_backtrace
0
```

#### hung_task_panic

配置检测到一个 hung 住的任务时内核的行为，当`CONFIG_DETECT_HUNG_TASK`内核配置开启时此项配置才会存在。

|value|含义|
|:-:|:-:|
|0|内核继续执行（默认配置）|
|1|立刻触发 panic|

```
❯ cat /proc/sys/kernel/hung_task_panic
0

```

#### hardlockup_panic

配置在检测到 hard lockup 产生时是否触发内核 panic。

| value |    含义     |
| :---: | :-------: |
|   0   | 不触发 panic |
|   1   | 触发 panic  |


```shell
❯ cat /proc/sys/kernel/hung_task_panic
0
```

#### hung_task_timeout_secs

当一个任务处于 D 状态且超过了此项配置的时间未被调度时内核会告警，
当CONFIG_DETECT_HUNG_TASK内核配置开启时此项配置才会存在。

默认值 120

|value|含义|
|:-:|:-:|
|0|无限超时时间，不会做任何检查|
|0:LONG_MAX / HZ|按照配置时间进行检查|


```shell
❯ cat /proc/sys/kernel/hung_task_timeout_secs
120
```


#### hung_task_panic

配置检测到一个 hung 住的任务时内核的行为，
当CONFIG_DETECT_HUNG_TASK内核配置开启时此项配置才会存在。

| value |      含义      |
| :---: | :----------: |
|   0   | 内核继续执行（默认配置） |
|   1   |  立刻触发 panic  |


#### watchdog

该参数可用于同时配置 soft lockup 检测器与 NMI watchdog 的关闭与开启 。



> https://blog.csdn.net/Longyu_wlz/article/details/126565853
