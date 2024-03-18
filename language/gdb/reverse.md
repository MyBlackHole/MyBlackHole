# reverse

```shell
- reverse-next(rc): 参考next(n), 逆向执行一行代码，遇函数调用不进入
- reverse-nexti(rni): 参考nexti(ni), 逆向执行一条指令，与函数调用不进入
- reverse-step(rs): 参考step(s), 逆向执行一行代码，遇函数调用则进入
- reverse-stepi(rsi): 参考setpi(si)， 逆向执行一条指令，与函数调用则进入
- reverse-continue(rc): 参考continue(c), 逆向继续执行
- reverse-finish: 参考finish，逆向执行，一直到函数入口处
- reverse-search(): 参考search，逆向搜索
- set exec-direction reverse: 设置程序逆向执行，执行完此命令后，所有常用命令如next, nexti, step, stepi, continue、finish等全部都变成逆向执行
- set exec-direction forward: 设置程序正向执行，这也是默认的设置
```



在绝大多数环境下，在使用这些反向调试命令之前，必须要先通过record命令让GDB把程序执行过程中的所有状态信息全部记录下来。
```shell
- record: 记录程序执行过程中所有状态信息
- record stop: 停止记录状态信息
- record goto: 让程序跳转到指定的位置, 如record goto start、record goto end、record goto n
- record save filename: 把程序执行历史状态信息保存到文件，默认名字是gdb_record.process_id
- record restore filename: 从历史记录文件中恢复状态信息
- show record full insn-number-max：查看可以记录执行状态信息的最大指令个数，默认是200000
- set record full insn-number-max limit：设置可以记录执行状态信息的最大指令个数
- set record full insn-number-max unlimited：记录所有指令的执行状态信息
```
