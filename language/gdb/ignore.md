# ignore

ignore 命令可以使目标断点暂时失去作用，当断点失效的次数超过指定次数时，断点的功能会自动恢复

ignore bnum count

- 
```shell
ignore 1 3
使编号为 1 的断点失效 3 次后
继续执行程序
最终程序仍暂停至编号 1 断点
```
