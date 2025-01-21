# rd

-p: 读物理地址
-u: 读用户虚拟地址
-d: 显示十进制，-x显示十六进制
-s: 显示符号
-32: 显示32位宽的值
-64: 显示64位宽的值
-a: 显示ASCII码

```shell
rd <address>

rd [addr] [len] # 查看指定地址，长度为len的内存
rd -S [addr][len] # 尝试将地址转换为对应的符号
rd [addr] -e [addr] # 查看指定内存区域内容
```
