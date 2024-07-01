# gcc

## 简介
GNU Compiler Collection (GCC) 是一套开源的、跨平台的编译器套件，它包含了 C、C++、Fortran、Java、Ada、Go、Pascal、Objective-C、Perl、Python、Ruby、Swift 等语言的编译器。

编译过程：预处理 -> 编译 -> 汇编 -> 链接 -> 优化 -> 输出

1. 预处理：预处理器（cpp）将源代码中的宏定义、条件编译指令、头文件包含、类型定义等进行处理，生成预处理后的源代码。
2. 编译：编译器（cc1、cc1plus、cc）将预处理后的源代码编译成汇编代码。
3. 汇编：汇编器（as）将汇编代码转换成机器代码。
4. 链接：链接器（ld）将多个目标文件和库文件链接成一个可执行文件。
5. 优化：优化器（opt）对生成的机器代码进行优化。

例如：
```shell
<!-- lvim test.c -->
int main(void)  
{
        printf("Hello World\n");  
        return 0;  
}

<!-- 预处理 -->
<!-- 这里主要负责展开在源文件中定义的宏，并向其中插入#include语句所包含的内容 -->
gcc -E test.c -o test.i

<!-- 编译 -->
<!-- 这里将预处理后的源代码编译成汇编代码 -->
gcc -S test.i -o test.s

<!-- 汇编 -->
<!-- 这里将汇编代码转换成机器代码 -->
gcc -c test.s -o test.o

<!-- 链接 -->
<!-- 这里将多个目标文件和库文件链接成一个可执行文件 -->
gcc test.o -o test

<!-- 优化 -->
<!-- 这里对生成的机器代码进行优化 -->
gcc -O2 test.o -o test

```


## 语法

-Wl,-rpath,/xxxx/lib (推荐)
-Wl,--dynamic-linker编译参数
设置LD_LIBRARY_PATH
设置PATH

- __builtin_expect
```
这个指令是gcc引入的，作用是允许程序员将最有可能执行的分支告诉编译器。这个指令的写法为：__builtin_expect(EXP, N)。
意思是：EXP==N的概率很大。

#define likely(x) __builtin_expect(!!(x), 1) //x很可能为真       
#define unlikely(x) __builtin_expect(!!(x), 0) //x很可能为假

if(likely(value))  //等价于 if(value)
if(unlikely(value))  //也等价于 if(value)

int x, y;
 if(unlikely(x > 0))
    y = 1; 
else 
    y = -1;
```
