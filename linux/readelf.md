# readelf

读取 elf 信息

## elf 文件类型
1. 可重定位文件: 用户和其他目标文件一起创建可执行文件或者共享目标文件,例如 lib*.a 文件。
2. 可执行文件： 用于生成进程映像，载入内存执行,例如编译好的可执行文件 a.out 。
3. 共享目标文件： 用于和其他共享目标文件或者可重定位文件一起生成 elf 目标文件或者和执行文件一起创建进程映像，例如 lib*.so 文件。

## elf 文件组成

elf 文件头描述 elf 文件的总体信息。包括： 系统相关，类型相关，加载相关，链接相关。

- 系统相关表示:elf 文件标识的魔术数，以及硬件和平台等相关信息，增加了 elf 文件的移植性,使交叉编译成为可能。
- 类型相关: 就是前面说的那个类型。
- 加载相关: 包括程序头表相关信息。
- 链接相关: 节头表相关信息。

##
-a , --all 显示全部信息,等价于 -h -l -S -s -r -d -V -A -I 。
-h , --file-header 显示 elf 文件开始的文件头信息.
-l , --program-headers , --segments 显示程序头（段头）信息(如果有的话)。
-S , --section-headers , --sections 显示节头信息(如果有的话)。
-g , --section-groups 显示节组信息(如果有的话)。
-t , --section-details 显示节的详细信息( -S 的)。
-s , --syms , --symbols 显示符号表段中的项（如果有的话）。
-e , --headers 显示全部头信息，等价于: -h -l -S
-n , --notes 显示 note 段（内核注释）的信息。
-r , --relocs 显示可重定位段的信息。
-u , --unwind 显示 unwind 段信息。当前只支持 IA64 ELF 的 unwind 段信息。
-d , --dynamic 显示动态段的信息。
-V , --version-info 显示版本段的信息。
-A , --arch-specific 显示 CPU 构架信息。
-D , --use-dynamic 使用动态段中的符号表显示符号，而不是使用符号段。
-x , --hex-dump= 以16进制方式显示指定段内内容。 number 指定段表中段的索引,或字符串指定文件中的段名。
-w[liaprmfFsoR] or –debug-dump[=line,=info,=abbrev,=pubnames,=aranges,=macro,=frames,=frames-interp,=str,=loc,=Ranges] 显示调试段中指定的内容。
-I , --histogram 显示符号的时候，显示 bucket list 长度的柱状图。
-v , --version 显示 readelf 的版本信息。
-H , --help 显示 readelf 所支持的命令行选项。
-W , --wide 宽行输出。

## 例子
- 获取符号信息
```shell
readelf -s /lib/x86_64-linux-gnu/libc.so.6
```

- 获取 elf 头
```shell
readelf -h /lib/x86_64-linux-gnu/libc.so.6
```
