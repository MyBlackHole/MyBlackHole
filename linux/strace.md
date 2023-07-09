# strace
strace是有用的诊断，说明和调试工具，Linux系统管理员可以在不需要源代码的情况下即可跟踪系统的调用
strace显示有关进程的系统调用的信息，这可以帮助确定一个程序使用的哪个函数，当然在系统出现问题时可以使用 strace定位系统调用过程中失败的原因，这是定位系统问题的很好的方法

## 参数
-c 统计每一系统调用的所执行的时间,次数和出错的次数等. 
-d 输出strace关于标准错误的调试信息. 
-f 跟踪由fork调用所产生的子进程. 
-ff 如果提供-o filename,则所有进程的跟踪结果输出到相应的filename.pid中,pid是各进程的进程号. 
-F 尝试跟踪vfork调用.在-f时,vfork不被跟踪. 
-h 输出简要的帮助信息. 
-i 输出系统调用的入口指针. 
-q 禁止输出关于脱离的消息. 
-r 打印出相对时间关于,,每一个系统调用. 
-t 在输出中的每一行前加上时间信息. 
-tt 在输出中的每一行前加上时间信息,微秒级. 
-ttt 微秒级输出,以秒了表示时间. 
-T 显示每一调用所耗的时间. 
-v 输出所有的系统调用.一些调用关于环境变量,状态,输入输出等调用由于使用频繁,默认不输出. 
-V 输出strace的版本信息. 
-x 以十六进制形式输出非标准字符串 
-xx 所有字符串以十六进制形式输出. 
-a column 
设置返回值的输出位置.默认 为40. 
-e expr 
指定一个表达式,用来控制如何跟踪.格式如下: 
[qualifier=][!]value1[,value2]... 
qualifier只能是 trace,abbrev,verbose,raw,signal,read,write其中之一.value是用来限定的符号或数字.默认的 qualifier是 trace.感叹号是否定符号.例如: 
-eopen等价于 -e trace=open,表示只跟踪open调用.而-etrace!=open表示跟踪除了open以外的其他调用.有两个特殊的符号 all 和 none. 
注意有些shell使用!来执行历史记录里的命令,所以要使用\\. 
-e trace= 
只跟踪指定的系统 调用.例如:-e trace=open,close,rean,write表示只跟踪这四个系统调用.默认的为set=all. 
-e trace=file 
只跟踪有关文件操作的系统调用. 
-e trace=process 
只跟踪有关进程控制的系统调用. 
-e trace=network 
跟踪与网络有关的所有系统调用. 
-e strace=signal 
跟踪所有与系统信号有关的 系统调用 
-e trace=ipc 
跟踪所有与进程通讯有关的系统调用 
-e abbrev= 
设定 strace输出的系统调用的结果集.-v 等与 abbrev=none.默认为abbrev=all. 
-e raw= 
将指 定的系统调用的参数以十六进制显示. 
-e signal= 
指定跟踪的系统信号.默认为all.如 signal=!SIGIO(或者signal=!io),表示不跟踪SIGIO信号. 
-e read= 
输出从指定文件中读出 的数据.例如: 
-e read=, 
-e write= 
输出写入到指定文件中的数据. 
-o filename 
将strace的输出写入文件filename 
-p pid 
跟踪指定的进程pid. 
-s strsize 
指定输出的字符串的最大长度.默认为32.文件名一直全部输出. 
-u username 
以username 的UID和GID执行被跟踪的命令

## 例子

```shell
strace -e open -o strace.out /usr/sbin/sshd
```

```shell
root@black:/media/black/z0# strace cat /dev/urandom >> 1.txt
execve("/usr/bin/cat", ["cat", "/dev/urandom"], 0x7ffcd2060d48 /* 41 vars */) = 0
brk(NULL)                               = 0x55c6477c9000
arch_prctl(0x3001 /* ARCH_??? */, 0x7fffc252be90) = -1 EINVAL (无效的参数)
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f596c8b2000
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (没有那个文件或目录)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
newfstatat(3, "", {st_mode=S_IFREG|0644, st_size=168997, ...}, AT_EMPTY_PATH) = 0
mmap(NULL, 168997, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f596c888000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0P<\2\0\0\0\0\0"..., 832) = 832
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 784, 64) = 784
newfstatat(3, "", {st_mode=S_IFREG|0644, st_size=2072888, ...}, AT_EMPTY_PATH) = 0
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 784, 64) = 784
mmap(NULL, 2117488, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f596c600000
mmap(0x7f596c622000, 1540096, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x22000) = 0x7f596c622000
mmap(0x7f596c79a000, 360448, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x19a000) = 0x7f596c79a000
mmap(0x7f596c7f2000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1f1000) = 0x7f596c7f2000
mmap(0x7f596c7f8000, 53104, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f596c7f8000
close(3)                                = 0
mmap(NULL, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f596c885000
arch_prctl(ARCH_SET_FS, 0x7f596c885740) = 0
set_tid_address(0x7f596c885a10)         = 970741
set_robust_list(0x7f596c885a20, 24)     = 0
rseq(0x7f596c886060, 0x20, 0, 0x53053053) = 0
mprotect(0x7f596c7f2000, 16384, PROT_READ) = 0
mprotect(0x55c645e57000, 4096, PROT_READ) = 0
mprotect(0x7f596c8e7000, 8192, PROT_READ) = 0
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
munmap(0x7f596c888000, 168997)          = 0
getrandom("\xd9\x1f\x96\x78\x0a\x6f\x99\xcc", 8, GRND_NONBLOCK) = 8
brk(NULL)                               = 0x55c6477c9000
brk(0x55c6477ea000)                     = 0x55c6477ea000
openat(AT_FDCWD, "/usr/lib/locale/locale-archive", O_RDONLY|O_CLOEXEC) = 3
newfstatat(3, "", {st_mode=S_IFREG|0644, st_size=8884624, ...}, AT_EMPTY_PATH) = 0
mmap(NULL, 8884624, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f596bc00000
close(3)                                = 0
newfstatat(1, "", {st_mode=S_IFREG|0644, st_size=2031681536, ...}, AT_EMPTY_PATH) = 0
openat(AT_FDCWD, "/dev/urandom", O_RDONLY) = 3
newfstatat(3, "", {st_mode=S_IFCHR|0666, st_rdev=makedev(0x1, 0x9), ...}, AT_EMPTY_PATH) = 0
fadvise64(3, 0, 0, POSIX_FADV_SEQUENTIAL) = 0
mmap(NULL, 139264, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f596c890000
read(3, "B\311\236\211\1\217\365\214\277\235o\342\223\373\336\331g\315U\367\317\204t\273\253\r\20T\244\341\323\364"..., 131072) = 131072
write(1, "B\311\236\211\1\217\365\214\277\235o\342\223\373\336\331g\315U\367\317\204t\273\253\r\20T\244\341\323\364"..., 131072) = -1 ENOSPC (设备上没有空间)
openat(AT_FDCWD, "/usr/share/locale/locale.alias", O_RDONLY|O_CLOEXEC) = 4
newfstatat(4, "", {st_mode=S_IFREG|0644, st_size=2996, ...}, AT_EMPTY_PATH) = 0
read(4, "# Locale name alias data base.\n#"..., 4096) = 2996
read(4, "", 4096)                       = 0
close(4)                                = 0
openat(AT_FDCWD, "/usr/share/locale/zh_CN/LC_MESSAGES/coreutils.mo", O_RDONLY) = -1 ENOENT (没有那个文件或目录)
openat(AT_FDCWD, "/usr/share/locale/zh/LC_MESSAGES/coreutils.mo", O_RDONLY) = -1 ENOENT (没有那个文件或目录)
openat(AT_FDCWD, "/usr/share/locale-langpack/zh_CN/LC_MESSAGES/coreutils.mo", O_RDONLY) = 4
newfstatat(4, "", {st_mode=S_IFREG|0644, st_size=359385, ...}, AT_EMPTY_PATH) = 0
mmap(NULL, 359385, PROT_READ, MAP_PRIVATE, 4, 0) = 0x7f596c82d000
close(4)                                = 0
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/gconv/gconv-modules.cache", O_RDONLY) = 4
newfstatat(4, "", {st_mode=S_IFREG|0644, st_size=27028, ...}, AT_EMPTY_PATH) = 0
mmap(NULL, 27028, PROT_READ, MAP_SHARED, 4, 0) = 0x7f596c889000
close(4)                                = 0
futex(0x7f596c7f7a6c, FUTEX_WAKE_PRIVATE, 2147483647) = 0
write(2, "cat: ", 5cat: )                    = 5
write(2, "\345\206\231\345\205\245\351\224\231\350\257\257", 12写入错误) = 12
openat(AT_FDCWD, "/usr/share/locale/zh_CN/LC_MESSAGES/libc.mo", O_RDONLY) = -1 ENOENT (没有那个文件或目录)
openat(AT_FDCWD, "/usr/share/locale/zh/LC_MESSAGES/libc.mo", O_RDONLY) = -1 ENOENT (没有那个文件或目录)
openat(AT_FDCWD, "/usr/share/locale-langpack/zh_CN/LC_MESSAGES/libc.mo", O_RDONLY) = 4
newfstatat(4, "", {st_mode=S_IFREG|0644, st_size=130962, ...}, AT_EMPTY_PATH) = 0
mmap(NULL, 130962, PROT_READ, MAP_PRIVATE, 4, 0) = 0x7f596c80d000
close(4)                                = 0
write(2, ": \350\256\276\345\244\207\344\270\212\346\262\241\346\234\211\347\251\272\351\227\264", 23: 设备上没有空间) = 23
write(2, "\n", 1
)                       = 1
close(1)                                = 0
close(2)                                = 0
exit_group(1)                           = ?
+++ exited with 1 +++
```
