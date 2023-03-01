### 描述


### 语法
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
