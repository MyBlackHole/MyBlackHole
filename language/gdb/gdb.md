# gcc
编译工具

## 参数

- follow-fork-mode detach-on-fork 
```
parent                   on               只调试主进程（GDB默认）
child                    on               只调试子进程
parent                   off              同时调试两个进程，gdb跟主进程，子进程block在fork位置
child                    off              同时调试两个进程，gdb跟子进程，主进程block在fork位置
```

### 常用命令
- list(l) -：查看当前
- list(l) <linenum>：显示 binFile linenum 附近源代码，接着上次的位置往下列，每次列10行。
- list(l) <function>：列出某个函数的源代码。
- r或run：运行程序。
- s或step：进入函数调用
- backtrace(bt) <n>：查看栈各级函数调用及参数
- frame(f) <n>：切换栈
- up(n) <n>：表示向栈的上面移动n层，可以不打n，表示向上移动一层
- down <n>：表示向栈的下面移动n层，可以不打n，表示向下移动一层
- info（i) locals：查看当前栈帧局部变量的值
- info break ：查看断点信息。
- finish：执行到当前函数返回，然后挺下来等待命令
- print(p)：打印表达式的值，通过表达式可以修改变量的值或者调用函数
- set var：修改变量的值
- quit：退出gdb
- break(b) 行号：在某一行设置断点
- jump(j) 行号: 让程序执行流程**跳转**到指定位置
- break 函数名：在某个函数开头设置断点
- continue(c)：从当前位置开始连续而非单步执行程序
- run(r)：从开始连续而非单步执行程序
- delete breakpoints：删除所有断点
- delete breakpoints n：删除序号为n的断点
- disable breakpoints：禁用断点
- enable breakpoints：启用断点
- info(i) args：打印出当前函数的参数名及其值
- info(i) locals：打印出当前函数中所有局部变量及其值
- info(i) catch:打印出当前的函数中的异常处理信息
- info(i) breakpoints:参看当前设置了哪些断点
- display 变量名:跟踪查看一个变量，每次停下来都显示它的值
- undisplay：取消对先前设置的那些变量的跟踪
- until <linenum>:脱离，跳至 linenum 行
- p 变量：打印变量值
- n 或 next：单条执行
- inferiors: 切换进程id(info inferiors 为 NUM)
- directory(dir): 设置源码目录

### 例子
- 设置多进程调试
```shell
set follow-fork-mode child
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

- 指定文件下断点
```shell
# 相对路径
break sys_socket_learn/shutdown_test.c:66
```

- 函数调用
```shell
call close(3)
```

- 预编译
```shell
gcc -E macro.c
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

- 查看字符串
```shell
x /s &ch
p (char*)&ch
```

- 制定 gdbinit