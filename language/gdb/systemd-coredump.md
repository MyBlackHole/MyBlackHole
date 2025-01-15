# systemd-coredump

调试应用程序崩溃

systemd-coredump 收集并显示内核核心转储，以分析应用程序崩溃。
当某个进程崩溃（或所有属于某个应用程序的进程）时，其默认设置是将核心转储记录到日志中（systemd如果可能的话包括回溯），
并将核心转储存储在中的文件中 /var/lib/systemd/coredump。
您还可以选择使用其他工具（例如gdb或crash） 检查转储文件。
可以选择不存储核心转储，而仅记录日志，这对于最大程度地减少敏感信息的收集和存储很有

- core 生成位置
```shell
cat /proc/sys/kernel/core_pattern
```

- 配置
```shell
cat /proc/sys/kernel/core_pattern
|/usr/lib/systemd/systemd-coredump %P %u %g %s %t %c %h


lvim /etc/systemd/coredump.conf
[Coredump]
#Storage=external
#Compress=yes
#ProcessSizeMax=2G
#ExternalSizeMax=2G
#JournalSizeMax=767M
#MaxUse=
#KeepFree=

lz4 -d core.lz4 > core

gdb app core
```
