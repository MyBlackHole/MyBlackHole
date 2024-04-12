# condition

```shell
与 break if 类似，
只是 condition 只能用在已存在的断点上。
用法：
condition <break_list> (condition)
例如：
cond 3 i == 3
将会在断点3上附加条件（i == 3）

condition bnum expression
condition bnum

condition 命令的功能是：
    既可以为现有的普通断点
    观察断点以及捕捉断点添加条件表达式
    也可以对条件断点的条件表达式进行修改
```
