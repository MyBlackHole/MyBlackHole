# gcc
编译工具

## 参数

- backtrace/bt
    打印当前的函数调用栈的所有信息

- frame/f <n>
    切换当前栈

- up <n>
    表示向栈的上面移动n层，可以不打n，表示向上移动一层

- down <n>
    表示向栈的下面移动n层，可以不打n，表示向下移动一层

- list <linenum>
    显示程序第linenum行的周围的源程序

- list <function>
    显示函数名为function的函数的源程序

- list 
    显示当前行后面的源程序

- list -
    显示当前行前面的源程序

- info args
    打印出当前函数的参数名及其值。

- info locals
    打印出当前函数中所有局部变量及其值。

- info catch
    打印出当前的函数中的异常处理信息。

## 例子
- 预编译
```shell
gcc -E macro.c
```

- 调试 pid 进程
```shell
gdb attach [pid] 
sudo gdb attach 766047
```

- 制定 gdbinit
