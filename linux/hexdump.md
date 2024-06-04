# hexdump

hexdump 是 Linux 下的一个二进制文件查看工具
它可以将二进制文件转换为ASCII、八进制、十进制、十六进制格式进行查看


```shell
hexdump: [-bcCdovx] [-e fmt] [-f fmt_file] [-n length] [-s skip] [file ...]
```


| 参数 | 描叙 |
| ---- | ---- |
| -b | 每个字节显示为8进制。一行共16个字节，一行开始以十六进制显示偏移值 |
| -c | 每个字节显示为ASCII字符 |
| -C | 每个字节显示为16进制和相应的ASCII字符 |
| -d | 两个字节显示为10进制 |
| -e | 格式化输出 |
| -f | Specify a file that contains one or more newline separated format strings.  Empty lines and lines whose first non-blank character is a hash mark (#) are ignored. |
| -n | 只格式前n个长度的字符 |
| -o | 两个字节显示为8进制 |
| -s | 从偏移量开始输出 |
| -v | The -v option causes hexdump to display all input data.  Without the -v option, any number of groups of output lines, which would be identical to the immediately preceding group of output lines |
| -x | 双字节十六进制显示 |



```shell
hexdump -C test.txt 
00000000  41 42 43 44 45 46 47 48  49 4a 4b 4c 4d 4e 4f 44  |ABCDEFGHIJKLMNOD|
00000010  46 31 32 2a 44 46 44 46  0a                       |F12*DFDF.|
```

- 查看指定大小的二进制文件
```shell
hexdump -C -n 8096 2662 
[postgres@pg02 818978]$ hexdump -C -n 8096 2662
00000000  00 00 00 00 a8 55 63 01  22 e6 00 00 40 00 f0 1f  |.....Uc."...@...|
00000010  f0 1f 04 20 00 00 00 00  62 31 05 00 04 00 00 00  |... ....b1......|
00000020  03 00 00 00 01 00 00 00  03 00 00 00 01 00 00 00  |................|
00000030  00 00 00 00 00 00 00 00  00 00 00 00 00 b0 78 40  |..............x@|
00000040  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
*
00001fa0


[postgres@pg02 818978]$ hexdump -C -n 8096 2662
00000000  00 00 00 00 a8 55 63 01  22 e6 00 00 40 00 f0 1f  |.....Uc."...@...|
00000010  f0 1f 04 20 00 00 00 00  62 31 05 00 04 00 00 00  |... ....b1......|
00000020  03 00 00 00 01 00 00 00  03 00 00 00 01 00 00 00  |................|
00000030  00 00 00 00 00 00 00 00  00 00 00 00 00 b0 78 40  |..............x@|
00000040  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
*
00001fa0
```

- 查看指定偏移量指定大小的二进制文件
```shell
hexdump -s 8192 -n 8192 16426
```
