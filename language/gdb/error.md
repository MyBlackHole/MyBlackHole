# error 

- Thread debugging using libthread_db enabled
```shell
正常提示,说明启用了线程调试
```

- <optimized out>
```shell
-O0：这个等级（字母“O”后面跟个零）关闭所有优化选项，也是CFLAGS或CXXFLAGS中没有设置-O等级时的默认等级。这样就不会优化代码，这通常不是我们想要的。
-O1：这是最基本的优化等级。编译器会在不花费太多编译时间的同时试图生成更快更小的代码。这些优化是非常基础的，但一般这些任务肯定能顺利完成。
-O2：-O1的进阶。这是推荐的优化等级，除非你有特殊的需求。-O2会比-O1启用多一些标记。设置了-O2后，编译器会试图提高代码性能而不会增大体积和大量占用的编译时间。
-O3：这是最高最危险的优化等级。用这个选项会延长编译代码的时间，并且在使用gcc4.x的系统里不应全局启用。自从3.x版本以来gcc的行为已经有了极大地改变。在3.x，-O3生成的代码也只是比-O2快一点点而已，而gcc4.x中还未必更快。用-O3来编译所有的软件包将产生更大体积更耗内存的二进制文件，大大增加编译失败的机会或不可预知的程序行为（包括错误）。这样做将得不偿失，记住过犹不及。在gcc 4.x.中使用-O3是不推荐的。
-Os：这个等级用来优化代码尺寸。其中启用了-O2中不会增加磁盘空间占用的代码生成选项。这对于磁盘空间极其紧张或者CPU缓存较小的机器非常有用。但也可能产生些许问题，因此软件树中的大部分ebuild都过滤掉这个等级的优化。使用-Os是不推荐的。
正如前面所提到的，-O2是推荐的优化等级。如果编译软件出现错误，请先检查是否启用了-O3。再试试把CFLAGS和CXXFLAGS倒回到较低的等级，如-O1甚或-O0 -g2 -ggdb（用来报告错误和检查可能存在的问题），再重新编译。
-O0 不进行优化处理。
-O 或 -O1 优化生成代码。
-O2 进一步优化。
-O3 比 -O2 更进一步优化，包括 inline 函数。
```

- Target multi-thread does not support this command. (执行反向调试)
```shell
<!-- 需要激活指令记录 -->
record
```

-   89521 segmentation fault (core dumped)  ./src/bin/pg_waldump/pg_waldump
```shell
[29627.947755] pg_waldump[89521]: segfault at 59792a8e5106 ip 0000597929b0114a sp 00007ffd75d8e6a0 error 4 in pg_waldump[597929aff000+16000] likely on CPU 4 (core 2, socket 0)
[29627.947795] Code: 45 b0 c7 00 00 00 00 00 48 8b 45 10 48 c7 00 00 00 00 00 0f b6 45 d4 83 e0 40 85 c0 74 3d 48 8b 45 d8 48 89 45 f0 48 8b 45 f0 <0f> b7 00 0f b7 d0 48 8b 45 18 89 10 48 8b 45 f0 48 8d 50 02 48 8b

<!-- ??? -->
objdump -d ./src/bin/pg_waldump/pg_waldump | 59792a8e5106 
```

- 
Cannot find thread-local storage for process 2752030, shared library /lib64/libc.so.6:
Cannot find thread-local variables on this target
```shell
<!-- pthread 被 stripped 了 -->
file /usr/lib64/libthread_db-1.0.so
/usr/lib64/libthread_db-1.0.so: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, BuildID[sha1]=48ba3467af87cece145819e2ccef090b1166053a, for GNU/Linux 3.2.0, stripped

file /usr/lib64/libpthread-2.28.so
/usr/lib64/libpthread-2.28.so: ELF 64-bit LSB shared object, x86-64, version 1 (GNU/Linux), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=d2cf4af642fe88c5343e1dc83dfef2e5d4466a68, for GNU/Linux 3.2.0, stripped
```
