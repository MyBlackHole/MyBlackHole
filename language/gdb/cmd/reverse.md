# reverse

回退

- record
回放记录

- record stop
关闭回放

- reverse-continue
反向运行程序知道遇到一个能使程序中断的事件（比如断点，观察点，异常）。

- reverse-step
反向运行程序到上一次被执行的源代码行。

- reverse-stepi
反向运行程序到上一条机器指令

- reverse-next
反向运行到上一次被执行的源代码行，但是不进入函数。

- reverse-nexti
反向运行到上一条机器指令，除非这条指令用来返回一个函数调用、整个函数将会被反向执行。

- reverse-finish
反向运行程序回到调用当前函数的地方。

- set exec-direction [forward | reverse]
设置程序运行方向，可以用平常的命令step和continue等来执行反向的调试命令。
