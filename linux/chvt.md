# chvt
修改虚拟终端的前台环境 

使用chvt命令在各个虚拟终端之间切换，chvt 可以看成是change virtual terminal的缩写，所以，chvt只能在各个虚拟终端之间切换，并不能在pts和tty之间切换，所以不要在ssh远程工具中执行chvt命令，也不要在vnc的显示界面中执行chvt命令，因为ssh远程工具和vnc远程工具打开的终端都是pts类型的模拟终端

## 例子
- 切换 1
```shell
Chvt 1
```