# error 

- <optimized out>
将GCC优化选项调整为O1或是O0
GCC在O2、O3优化选项下会将代码优化的比较多，调试器有可能会找不到变量的信息。通常可以将优化级别降低到O0，完全关闭优化，可以保留所有的变量和代码信息。使用O1优化有可能也可以看得到变量的值。

当然，这种直接降低优化级别的方法还是比较暴力的。如果你的程序比较大，运行到你需要调试的地方需要很久的情况下，通常都是行不通的，毕竟所有优化选项都关掉了，程序跑的会非常慢……

输出打印信息
直接将变量的信息打印到屏幕上是最直接的方法。当然了，这种方法太糙，太繁琐了。一旦你需要看的变量比较多，输出的内容也会非常多，看起来就比较痛苦了。

用volatile修饰需要显示的变量
在需要显示值的变量前面加上volatile修饰符也是一种比较管用的方法。这种方法不需要修改编译器的优化级别，对于比较庞大的程序来说是比较合适的。如果这种方法也不管用或是也不适用的话，请往下看

GCC下关闭某函数的优化选项