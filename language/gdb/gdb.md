# gdb
调试工具

```
https://github.com/cyrus-and/gdb-dashboard
```

```
### 常用命令
- file: 装入想要调试的可执行文件
- kill: 终止正在调试的程序
- list(l) -：查看当前
- list(l) <linenum>：显示 binFile linenum 附近源代码，接着上次的位置往下列，每次列10行。
- list(l) <function>：列出某个函数的源代码。
- run(r)：运行程序。
- step(s)：进入函数调用
- backtrace(bt) <n>：查看栈各级函数调用及参数
- frame(f) <n>：切换栈
- up(n) <n>：表示向栈的上面移动n层，可以不打n，表示向上移动一层
- down <n>：表示向栈的下面移动n层，可以不打n，表示向下移动一层
- finish：执行到当前函数返回，然后挺下来等待命令
- print(p)：打印表达式的值，通过表达式可以修改变量的值或者调用函数
- set var：修改变量的值
- quit：退出gdb
- jump(j) 行号: 让程序执行流程**跳转**到指定位置
- continue(c)：从当前位置开始连续而非单步执行程序
- run(r)：从开始连续而非单步执行程序
- delete breakpoints：删除所有断点
- delete breakpoints n：删除序号为n的断点
- disable breakpoints：禁用断点
- enable breakpoints：启用断点
- display 变量名:跟踪查看一个变量，每次停下来都显示它的值
- undisplay：取消对先前设置的那些变量的跟踪
- until <linenum>:脱离，跳至 linenum 行
- next(n)：单条执行
- inferiors: 切换进程id(info inferiors 为 NUM)
- directory(dir): 设置源码目录
- watch var: 观察变量, 只要变化
- whatis: 显示变量或函数类型
- call name(args): 调用并执行名为name，参数为args的函数
- return value: 停止当前函数并返回value给调用者
- ptype: 显示结构定义
- make: 使你能不退出gdb就可以重新产生可执行文件
- shell: 使你能不离开gdb就执行UNIX shell命令
```

### 例子
- 多线程调试
```shell


info threads   查看当前进程的线程。
    GDB会为每个线程分配一个ID, 后面操作线程的时候会用到这个ID.
    前面有*的是当前调试的线程.
thread <ID>  切换调试的线程为指定ID的线程。

break ChangeTrackup thread all  为所有经过 ChangeTrackup 的线程设置断点。
set scheduler-locking off|on|step    
      在使用step或者continue命令调试当前被调试线程的时候，其他线程也是同时执行的,
      怎么只让被调试程序执行呢？
      通过这个命令就可以实现这个需求。
         off      不锁定任何线程，也就是所有线程都执行，这是默认值。
         on       只有当前被调试程序会执行。
         step     在单步的时候，除了next过一个函数的情况
                  (熟悉情况的人可能知道，这其实是一个设置断点然后continue的行为)以外，
                  只有当前线程会执行。
thread apply ID1 ID2 command        让一个或者多个线程执行GDB命令command
thread apply all command            让所有被调试线程执行GDB命令command。
```

- 设置多进程调试
```shell
<!-- 调试子进程 -->
set follow-fork-mode child

<!-- 禁用 fork 进程 -->
set detach-on-fork off
```

- 传递参数
```shell
gdb --args ./test/unit/core configurationDecode/badAddress
# 或
(gdb)r arg1 arg2
# 或
(gdb)set args arg1 arg2
```

- 使用 .gdbinit
```
# gdb
set disassemble-next-line on
b _start
target remote : 1234
c

# shell
gdb -x .gdbinit ./test/unit/core
```

- 设置源码目录
```
(gdb) directory /media/black/Data/Documents/github/C/tbox
Source directories searched: /media/black/Data/Documents/github/C/tbox:/media/black/Data/Documents/C/C_learn/out/obj/tbox_learn/heap_test1:$cdir:$cwd
```

- 预编译
```shell
gcc -E macro.c
gcc -I./ -I./include -E -P  libbcachefs/opts.c > opts_test.h
```

- 调试 pid 进程
```shell
gdb attach [pid] 
sudo gdb attach 766047
```

- 调试 python 进程
```shell
gdb -p pid
source Tools/gdb/libpython.py # cpython 里有
```


- 制定 gdbinit
