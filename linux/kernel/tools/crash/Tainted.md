# Tainted

```shell
Pid: 0, comm: swapper Tainted: G      D ........

1: 'G': 所有的模块都是GPL的License。如果有模块缺少MODULE_LICENSE()或者声明是Proprietary的，则为'P'。
2: 'F': 如果有模块是使用 insmod -f 强制载入的。否则为空。
3: 'S': 如果Oops发生在SMP的CPU上，但这个型号的CPU还没有被认为是SMP安全的。
4: 'R': 如果有模块是使用 rmmod -f 强制卸载的。否则为空。
5: 'M': 有CPU报告了Machine Check Exception，否则为空。
6: 'B': 如果有page-release函数发现一个错误的page或未知的page标志。
7: 'U': 来自用户空间的程序设置的这个标志位。
8: 'D': 内核刚刚死掉，比如Oops或者是bug。
9: 'A': ACPI表被覆盖。
10: 'W': 之前kernel已经产生过警告。
```
