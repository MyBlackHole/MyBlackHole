# addr2line

-a --addresses：在函数名、文件和行号信息之前，显示地址，以十六进制形式。
-b --target=：指定目标文件的格式为bfdname。
-e --exe=：指定需要转换地址的可执行文件名。
-i --inlines ： 如果需要转换的地址是一个内联函数，则输出的信息包括其最近范围内的一个非内联函数的信息。
-j --section=：给出的地址代表指定section的偏移，而非绝对地址。
-p --pretty-print：使得该函数的输出信息更加人性化：每一个地址的信息占一行。
-s --basenames：仅仅显示每个文件名的基址（即不显示文件的具体路径，只显示文件名）。
-f --functions：在显示文件名、行号输出信息的同时显示函数名信息。
-C --demangle[=style]：将低级别的符号名解码为用户级别的名字。
-h --help：输出帮助信息。
-v --version：输出版本号。


```shell

[2024-01-31 12:34:36]|Info |main.cpp:51 logerr|./fsdeamon(+0x4ec4e) [0x5654975fac4e]
[2024-01-31 12:34:36]|Info |main.cpp:51 logerr|./fsdeamon(+0x4ec4e) [0x5654975fac4e]
[2024-01-31 12:34:36]|Info |main.cpp:51 logerr|./fsdeamon(+0x4e52e) [0x5654975fa52e]
[2024-01-31 12:34:36]|Info |main.cpp:51 logerr|./fsdeamon(+0x4e130) [0x5654975fa130]
[2024-01-31 12:34:36]|Info |main.cpp:51 logerr|./fsdeamon(+0x7b25b) [0x56549762725b]
[2024-01-31 12:34:36]|Info |main.cpp:51 logerr|./fsdeamon(+0x7befa) [0x565497627efa]
[2024-01-31 12:34:36]|Info |main.cpp:51 logerr|/lib/x86_64-linux-gnu/libc.so.6(+0x8f6ba) [0x7f955101e6ba]
[2024-01-31 12:34:36]|Info |main.cpp:51 logerr|/lib/x86_64-linux-gnu/libc.so.6(+0x11e120) [0x7f95510ad120]
[2024-01-31 12:34:36]|Error |main.cpp:75 sigshutdown|sign:6.(Success)
[2024-01-31 12:34:36]|Error |main.cpp:75 sigshutdown|sign:11.(Success)


addr2line -e ./fsdeamon -f 0x7befa
_ZN12FsKernelSync20CheckKernelModThreadEPv
/media/black/Data/Documents/AIO/fs-backup/fsdeamon/fs_kernel_sync.cpp:550


[54106.016179] test1[8352] trap divide error ip:400506 sp:7fff2add87e0 error:0 in test1[400000+1000]
addr2line -e test1 400506
/home/hanfoo/code/test/addr2line/test1.c:5
```
