# info

- info(i) breakpoints:参看当前设置了哪些断点
- info files: 显示被调试文件的详细信息
- info func: 显示所有的函数名称
- info(i) locals：查看当前栈帧局部变量的值
- info break ：查看断点信息。
- info prog: 显示被调试程序的执行状态
- info(i) args：打印出当前函数的参数名及其值
- info(i) locals：打印出当前函数中所有局部变量及其值
- info(i) catch:打印出当前的函数中的异常处理信息
- info(i) registers:打印出当前寄存器的值

## macro
宏 [[macro]]

- 显示 raxNodeFirstChildPtr 定义
```shell
info macro raxNodeFirstChildPtr
```

- 查看进程列表
```shell
(gdb) info inferiors 
  Num  Description       Connection           Executable        
* 1    process 185298    1 (native)           /run/media/black/Data/lib/postgres/zh_master/bin/postgres 
  2    process 185379    1 (native)           /run/media/black/Data/lib/postgres/zh_master/bin/postgres 
  3    process 185385    1 (native)           /run/media/black/Data/lib/postgres/zh_master/bin/postgres 
  4    process 185465    1 (native)           /run/media/black/Data/lib/postgres/zh_master/bin/postgres 
```

- 查看自动加载的脚本
```shell
(gdb) info auto-load
gdb-scripts:  No auto-load scripts.
guile-scripts:  No auto-load scripts.
libthread-db:  No auto-loaded libthread-db.
local-gdbinit:  Local .gdbinit file was not found.
python-scripts:
Loaded  Script
Yes     /run/media/black/Data/Documents/linux_debug/linux-4.19.315/vmlinux-gdb.py
```

- 查看寄存器
```shell
(gdb) i registers
rax            0x4030bf            4206783
rbx            0x2                 2
rcx            0x7fffffffd940      140737488345408
rdx            0x1                 1
rsi            0x7fffffffd940      140737488345408
rdi            0x1                 1
rbp            0x7fffffffd7d0      0x7fffffffd7d0
rsp            0x7fffffffd7a0      0x7fffffffd7a0
r8             0x110               272
r9             0x4                 4
r10            0x4a5750            4872016
r11            0xf                 15
r12            0x7fffffffd938      140737488345400
r13            0x7fffffffd950      140737488345424
r14            0x4cf7e0            5044192
r15            0x2                 2
rip            0x4030c7            0x4030c7 <demo_disassemble_main+8>
eflags         0x206               [ PF IF ]
cs             0x33                51
ss             0x2b                43
ds             0x0                 0
es             0x0                 0
fs             0x0                 0
gs             0x0                 0
fs_base        0x404dc380          1078838144
gs_base        0x0                 0
```
