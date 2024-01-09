# g

- 获取文件中当前字节位置
```shell
g + ctrl-g

<!-- 例如: -->
Col 33-23 of 35-24; Line 3 of 6; Word 4 of 9; Char 18 of 43; Byte 40 of 65

Byte 40 of 65: 最后两个数字就是文件中的当前字节位置和文件字节总数
```

- 获取光标下的字符值
```
ga

<!-- 例如: -->
<a>  97,  Hex 61,  Octal 141
```
