# unzip
解压缩 zip 压缩文件

## 语法
unzip [选项] [参数]

### 选项
-c：将解压缩的结果显示到屏幕上，并对字符做适当的转换；
-f：更新现有的文件；
-l：显示压缩文件内所包含的文件；
-p：与-c参数类似，会将解压缩的结果显示到屏幕上，但不会执行任何的转换；
-t：检查压缩文件是否正确；
-u：与-f参数类似，但是除了更新现有的文件外，也会将压缩文件中的其他文件解压缩到目录中；
-v：执行时显示详细的信息；
-z：仅显示压缩文件的备注文字；
-a：对文本文件进行必要的字符转换；
-b：不要对文本文件进行字符转换；
-C：压缩文件中的文件名称区分大小写；
-j：不处理压缩文件中原有的目录路径；
-L：将压缩文件中的全部文件名改为小写；
-M：将输出结果送到more程序处理；
-n：解压缩时不要覆盖原有的文件；
-o：不必先询问用户，unzip执行后覆盖原有的文件；
-P<密码>：使用zip的密码选项；
-q：执行时不显示任何信息；
-s：将文件名中的空白字符转换为底线字符；
-V：保留VMS的文件版本信息；
-X：解压缩时同时回存文件原来的UID/GID；
-d<目录>：指定文件解压缩后所要存储的目录；
-x<文件>：指定不要处理.zip压缩文件中的哪些文件；
-Z：unzip-Z等于执行zipinfo指令。

### 参数
压缩包：指定要解压的“.zip”压缩包

## 例子
- 解压到当前目录
```shell
unzip test.zip
```

- 解压到指定目录
```shell
unzip -n test.zip -d /tmp
```

- 查看目录信息（不解压）
```shell
unzip -v test.zip
```

- 覆盖解压
```shell
unzip -o test.zip -d /tmp
```

- 中文乱码
```shell
unzip -O GBK test.zip
unzip -O UTF-8 test.zip
```