# ldd (不推荐)
[[pmap]]
用于打印程序或者库文件所依赖的共享库列表。

## 语法
ldd(选项)(参数)

### 选项
--version：打印指令版本号；
-v：详细信息模式，打印所有相关信息；
-u：打印未使用的直接依赖；
-d：执行重定位和报告任何丢失的对象；
-r：执行数据对象和函数的重定位，并且报告任何丢失的对象和函数；
--help：显示帮助信息。

### 参数
文件：指定可执行程序或者文库。

## 例子
```shell
ldd ./out/obj/stdio_learn/print_time
        linux-vdso.so.1 (0x00007ffcbc348000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f0fbe53b000)
        /lib64/ld-linux-x86-64.so.2 (0x00007f0fbe75e000)

/lib/ld-linux.so.2 --list program

/sbin/ldconfig -p | grep stdc++
```
