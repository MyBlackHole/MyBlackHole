# hbreak

在嵌入式系统中，如果想调试的程序不是位于内存中，
而是位于像闪存这样的存储器中，此时就无法使用软件程序断点了，
因为闪存中的内容并不像内存那样方便更改。

此时只能使用硬件程序断点来调试程序。
硬件程序断点的实现原理与软件程序断点完全不同，断点时通过配置处理器的断点寄存器的方式来实现的。

当处理器运行到断点寄存器所指示位置的指令时就会产生中断，调试工具通过该中断使我们获得干预的机会。
处理器所能设置的硬件程序断点数量是有限的，可能最多也就4个。
 

```shell
<!-- linux 启动 debug -->
hbreak start_kernel
```
