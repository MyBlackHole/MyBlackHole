# PLT

Procedure Linkage Table
过程链接表的作用是将位置无关的符号转移到绝对地址
当一个外部符号被调用时，PLT 去引用 GOT 中的其符号对应的绝对地址，然后转入并执行
