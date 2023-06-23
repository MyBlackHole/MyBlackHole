# tar

文件打包压缩，解压提取

## 参数

|   |   |
|---|---|
|-A或--catenate|新增文件到以存在的备份文件；|
|-B|设置区块大小；|
|-c或--create|建立新的备份文件；|
|-C <目录>|这个选项用在解压缩，在特定目录解压缩|
|-d|记录文件的差别；|
|-x或--extract或--get|从备份文件中还原文件；|
|-t或--list|列出备份文件的内容；|
|-z或--gzip或--ungzip|通过gzip指令处理备份文件；|
|-Z或--compress或--uncompress|通过compress指令处理备份文件；|
|-f<备份文件>或--file=<备份文件>|指定备份文件；|
|-v或--verbose|显示指令执行过程；|
|-r|添加文件到已经压缩的文件；|
|-u|添加改变了和现有的文件到已经存在的压缩文件；|
|-j|支持bzip2解压文件；|
|-v|显示操作过程；|
|-l|文件系统边界设置；|
|-k|保留原有文件不覆盖；|
|-m|保留文件不被覆盖；|
|-w|确认压缩文件的正确性；|
|-p或--same-permissions|用原来的文件权限还原文件；|
|-P或--absolute-names|文件名使用绝对名称，不移除文件名称前的“/”号；|
|-N <日期格式> 或 --newer=<日期时间>|只将较指定日期更新的文件保存到备份文件里；|
|--exclude=<范本样式>|排除符合范本样式的文件。|

## 例子

|   |   |
|---|---|
|tar -cvf log.tar log2012.log|仅打包，不压缩|
|tar -zcvf log.tar.gz log2012.log|打包后，以 gzip 压缩|
|tar -jcvf log.tar.bz2 log2012.log|打包后，以 bzip2 压缩|
|查　询|tar -jtvf filename.tar.bz2|
|压　缩bz2|tar -jcvf file.tar.bz2 要被压缩的文件或目录|
|解压缩bz2|tar -jxvf filename.tar.bz2 -C 欲解压缩的目录|
|压　缩xz|tar caf 压缩包.tar.xz \*.txt（要压缩的文件）|
|解压缩xz|tar xf 压缩包.tar.xz|
|解压缩gz|tar -zxvf cmd_markdown_linux64.tar.gz|
|压　缩gz|tar -zcvf file.tar.gz 要被压缩的文件或目录|


- 解压 bz2
```shll
tar xfj xxxxxxxxxxxx.tar.bz2
```
