# ulimit

- 查看所有限制
```shell
ulimit -a
-t: cpu time (seconds)              unlimited
-f: file size (blocks)              unlimited
-d: data seg size (kbytes)          unlimited
-s: stack size (kbytes)             8192
-c: core file size (blocks)         0
-m: resident set size (kbytes)      unlimited
-u: processes                       60218
-n: file descriptors                1024
-l: locked-in-memory size (kbytes)  1945500
-v: address space (kbytes)          unlimited
-x: file locks                      unlimited
-i: pending signals                 60218
-q: bytes in POSIX msg queues       819200
-e: max nice                        0
-r: max rt priority                 0
-N 15: rt cpu time (microseconds)   unlimited
```

- 修改程序崩溃 core 大小
```shell
ulimit -c 1024

/proc/sys/kernel/core_pattern
|/usr/share/apport/apport -p%p -s%s -c%c -d%d -P%P -u%u -g%g -- %E
<!-- 修改  -->
echo "core-%p-%e" > /proc/sys/kernel/core_pattern
```
