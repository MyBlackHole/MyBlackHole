# gdb
调试工具

```
https://github.com/cyrus-and/gdb-dashboard
```

## 参数

- follow-fork-mode detach-on-fork 
```
parent                   on               只调试主进程（GDB默认）
child                    on               只调试子进程
parent                   off              同时调试两个进程，gdb跟主进程，子进程block在fork位置
child                    off              同时调试两个进程，gdb跟子进程，主进程block在fork位置
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
- print(p) var：打印变量值
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

### 例子
- 设置多进程调试
```shell
<!-- 调试进程 -->
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


- 制定 gdbinit
