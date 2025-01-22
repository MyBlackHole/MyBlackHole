# search 

搜索栈空间中的数据。

```shell
search [-s start] [ -[kKV] | -u | -p | -t | -T ] [-e end | -l length] [-m mask]
       [-x count] -[cwh] [value | (expression) | symbol | string] ...
```

-t: 从系统中所有的线程的栈空间里查找当前锁

- 搜索栈空间地址
```shell
search -t ffffb26601a97aa0
```
