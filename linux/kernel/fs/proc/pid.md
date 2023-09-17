# proc pid

- /proc/[pid]/auxv
包含传递给进程的 ELF 解释器信息，格式是每一项都是一个 unsigned long长度的 ID 加上一个 unsigned long 长度的值。最后一项以连续的两个 0x00 开头

- /proc/[pid]/cmdline
是一个只读文件，包含进程的完整命令行信息。如果该进程已经被交换出内存或者这个进程是 zombie 进程，则这个文件没有任何内容。该文件以空字符 null 而不是换行符作为结束标志

- /proc/[pid]/cwd
是进程当前工作目录的符号链接

- /proc/[pid]/environ
显示进程的环境变量

- /proc/[pid]/exe
为实际运行程序的符号链接

- /proc/[pid]/fd
是一个目录，包含进程打开文件的情况
目录中的每一项都是一个符号链接，指向打开的文件，数字则代表文件描述符

- /proc/[pid]/latency
显示哪些代码造成的延时比较大
如果要使用这个特性需要执行:
```shell
echo 1 > /proc/sys/kernel/latencytop
```
每一行前三个数字分别是后面代码执行的次数、总共执行延迟时间(单位是微秒)和最长执行延迟时间(单位是微秒)。后面则是代码完整的调用栈

- /proc/[pid]/maps
显示进程的内存区域映射信息
其中注意的一点是 [stack:] 是线程的堆栈信息，对应于 /proc/[pid]/task/[tid]/ 路径

- /proc/[pid]/root
是进程根目录的符号链接

- /proc/[pid]/stack
示当前进程的内核调用栈信息，只有内核编译时打开了 CONFIG_STACKTRACE 编译选项，才会生成这个文件

- /proc/[pid]/statm
显示进程所占用内存大小的统计信息。包含七个值，度量单位是 page(page大小可通过 getconf PAGESIZE 得到)
a. 进程占用的总的内存
b. 进程当前时刻占用的物理内存
c. 同其它进程共享的内存
d. 进程的代码段
e. 共享库(从2.6版本起，这个值为0)
f. 进程的堆栈
g. dirty pages(从2.6版本起，这个值为0)

- /proc/[pid]/status
包含进程的状态信息。其很多内容与 /proc/[pid]/stat 和 /proc/[pid]/statm 相同，但是却是以一种更清晰地方式展现出来

- /proc/[pid]/syscall
显示当前进程正在执行的系统调用
第一个值是系统调用号（202代表poll），后面跟着 6 个系统调用的参数值（位于寄存器中），最后两个值依次是堆栈指针和指令计数器的值。如果当前进程虽然阻塞，但阻塞函数并不是系统调用，则系统调用号的值为 -1，后面只有堆栈指针和指令计数器的值。如果进程没有阻塞，则这个文件只有一个 running 的字符串。

内核编译时打开了 CONFIG_HAVE_ARCH_TRACEHOOK 编译选项，才会生成这个文件

- /proc/[pid]/wchan
显示当进程 sleep 时，kernel 当前运行的函数
