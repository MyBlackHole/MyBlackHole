# sparse

Sparse 是内核代码静态分析工具, 能够帮助我们找出代码中的隐患.

| **宏名称**          | **宏定义**                                    | **检查点**                                       |
| ---------------- | ------------------------------------------ | --------------------------------------------- |
| __bitwise        | __attribute__((bitwise))                   | 确保变量是相同的位方式(比如 bit-endian, little-endiandeng) |
| __user           | __attribute__((noderef, address_space(1))) | 指针地址必须在用户地址空间                                 |
| __kernel         | __attribute__((noderef, address_space(0))) | 指针地址必须在内核地址空间                                 |
| __iomem          | __attribute__((noderef, address_space(2))) | 指针地址必须在设备地址空间                                 |
| __safe           | __attribute__((safe))                      | 变量可以为空                                        |
| __force          | __attribute__((force))                     | 变量可以进行强制转换                                    |
| __nocast         | __attribute__((nocast))                    | 参数类型与实际参数类型必须一致                               |
| __acquires(x)    | __attribute__((context(x, 0, 1)))          | 参数x 在执行前引用计数必须是0,执行后,引用计数必须为1                 |
| __releases(x)    | __attribute__((context(x, 1, 0)))          | 与 __acquires(x) 相反                            |
| __acquire(x)     | __context__(x, 1)                          | 参数x 的引用计数 + 1                                 |
| __release(x)     | __context__(x, -1)                         | 与 __acquire(x) 相反                             |
| __cond_lock(x,c) | ((c) ? ({ __acquire(x); 1; }) : 0)         | 参数c 不为0时,引用计数 + 1, 并返回1                       |

Sparse除了能够用在内核代码的静态分析上, 其实也可以用在一般的C语言程序中.

- 比如下面的小例子
```c
#include <stdio.h>

#define __acquire(x) __context__(x,1)
#define __release(x) __context__(x,-1)

int main(int argc, char *argv[])
{
    int lock = 1;
    __acquire(lock);
    /* TODO something */
    __release(lock);            /* 注释掉这一句 sparse 就会报错 */
    return 0;
}
```

- 如果安装了 Sparse, 执行静态检查的命令如下
```shell
sparse -a sparse_test.c 
sparse_test.c:15:5: warning: context imbalance in 'main' - wrong count at exit
```



