### 描述

### 参数

- follow-fork-mode detach-on-fork 
```
parent                   on               只调试主进程（GDB默认）
child                    on               只调试子进程
parent                   off              同时调试两个进程，gdb跟主进程，子进程block在fork位置
child                    off              同时调试两个进程，gdb跟子进程，主进程block在fork位置
```

### 常用命令
- list／l 行号：显示binFile源代码，接着上次的位置往下列，每次列10行。
- list／l 函数名：列出某个函数的源代码。
- r或run：运行程序。
- s或step：进入函数调用
- breaktrace（bt)：查看各级函数调用及参数
- info（i) locals：查看当前栈帧局部变量的值
- info break ：查看断点信息。
- finish：执行到当前函数返回，然后挺下来等待命令
- print(p)：打印表达式的值，通过表达式可以修改变量的值或者调用函数
- set var：修改变量的值
- quit：退出gdb
- break(b) 行号：在某一行设置断点
- jump(j) 行号: 让程序执行流程**跳转**到指定位置
- break 函数名：在某个函数开头设置断点
- continue(或c)：从当前位置开始连续而非单步执行程序
- run(或r)：从开始连续而非单步执行程序
- delete breakpoints：删除所有断点
- delete breakpoints n：删除序号为n的断点
- disable breakpoints：禁用断点
- enable breakpoints：启用断点
- info(或i) breakpoints：参看当前设置了哪些断点
- display 变量名：跟踪查看一个变量，每次停下来都显示它的值
- undisplay：取消对先前设置的那些变量的跟踪
- until X行号：跳至X行
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
