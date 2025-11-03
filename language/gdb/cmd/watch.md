# watch

## 用法
```shell
watch var

监视点的设定不依赖于断点的位置，但是与变量的作用域有关，也就是说，要设置监视点必须在程序运行时才可设置。

在不确定发生问题的地方时，通过使用监视点的条件表达式，可以非常方便地找出问题代码：

watch i > 999

一旦 i > 999，程序就会被中断，GDB指出改变条件的代码。

```

- 显示监视点的列表。
```shell
info watchpoints

```

- 删除监视点。
```shell
delete num
```

## 例子


```shell
(gdb) b main 
Breakpoint 1 at 0x80483ed: file test.c, line 7.
(gdb) watch b
No symbol "b" in current context.//现在还有变量b
(gdb) r
Starting program: /home/test/test 
 
Breakpoint 1, main () at test.c:7
7	  a = 1;
(gdb) watch b
Hardware watchpoint 2: b//已经进入main函数，变量b存在 
(gdb) c
Continuing.
Hardware watchpoint 2: b
 
Old value = 134513753//没有初始化b是随机值
New value = 2
main () at test.c:9
9	  c = 3;
(gdb) c
Continuing.
Hardware watchpoint 2: b
 
Old value = 2
New value = 0
0x08048416 in main () at test.c:10
10	  memset(&a,0,4*sizeof(int));//这里越界导致b，c被修改
(gdb) c
Continuing.
0 0 
```

- 地址内容变化监视点 (推荐)
```shell
(gdb) p m_backupTime
$1 = 0
(gdb) p &m_backupTime
$2 = (uint64_t *) 0x579fc10547c8
(gdb) watch *0x579fc10547c8 != 0
```

- errno
```gdb
(gdb) watch (int *)__errno_location()
```
