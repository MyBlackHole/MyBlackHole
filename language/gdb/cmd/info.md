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

