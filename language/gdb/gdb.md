# gdb

调试工具

## 安装
```
paru -S gdb

<!--插件-->
https://github.com/cyrus-and/gdb-dashboard
https://github.com/pwndbg/pwndbg
paru -S pwndbg
```


## 基本用法
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

