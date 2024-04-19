# inferiors

进程

## 相关参数
| follow-fork-mode | detach-on-fork | 说明 |
| ---- | ---- | ---- |
| parent | on | 只调试主进程( GDB 默认) |
| child | on | 只调试子进程 |
| parent | off | 同时调试两个进程, gdb 跟主进程, 子进程 block 在 fork 位置 |
| child | off | 同时调试两个进程, gdb 跟子进程, 主进程 block 在 fork 位置 |


- 切换进程
```shell
<!-- 例如 -->
(gdb) info inferiors 
  Num  Description       Connection           Executable        
* 1    process 185298    1 (native)           /run/media/black/Data/lib/postgres/zh_master/bin/postgres 
  2    process 185379    1 (native)           /run/media/black/Data/lib/postgres/zh_master/bin/postgres 
  3    process 185385    1 (native)           /run/media/black/Data/lib/postgres/zh_master/bin/postgres 
  4    process 185465    1 (native)           /run/media/black/Data/lib/postgres/zh_master/bin/postgres 

inferior 4
```


- 多进程 debug 流程
```shell
1. 确定需要 debug 的进程

2. break fork

3. 运行到 fork 后

4. info inferiors (查看启动了没)

5. set follow-fork-mode child (设置跟踪子进程)

6. break xxxxx (给所有进程下断点)

7. 看到自己需要的进程也被下了断点，可以开始 debug 了

1       breakpoint     keep y   0x00007ffff787b660 in __libc_fork at fork.c:41 inf 2
        breakpoint already hit 3 times
2       breakpoint     keep y   0x0000555555aaee88 in StartupProcessMain at startup.c:221 inf 2
3       breakpoint     keep y   <MULTIPLE>
3.1                         y   0x000000000055ae88 in StartupProcessMain at startup.c:221 inf 1
3.2                         y   0x0000555555aaee88 in StartupProcessMain at startup.c:221 inf 2



<!-- // 看上面，即开启调试多个进程，并且主进程 block 在 fork 位置 -->
<!-- set detach-on-fork off -->

```
