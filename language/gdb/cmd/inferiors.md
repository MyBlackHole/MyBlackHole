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


- 多进程 debug
```shell

break fork


// 设置默认跟踪子进程
set follow-fork-mode child
// 看上面，即开启调试多个进程，并且主进程 block 在 fork 位置
set detach-on-fork off

<!-- 查看启动了没 -->
info inferiors 

<!-- 给所有进程下断电 -->
b xxxxx

```
