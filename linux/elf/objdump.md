# objdump

- 符号表
```shell
objdump -t hello.ko

hello.ko:     file format elf64-x86-64

SYMBOL TABLE:
0000000000000000 l    d  .text  0000000000000000 .text
0000000000000000 l    d  .init.text     0000000000000000 .init.text
0000000000000000 l    d  .exit.text     0000000000000000 .exit.text
0000000000000000 l    d  .rodata.str1.1 0000000000000000 .rodata.str1.1
0000000000000000 l    d  .modinfo       0000000000000000 .modinfo
0000000000000000 l    d  .orc_unwind_ip 0000000000000000 .orc_unwind_ip
0000000000000000 l    d  .orc_unwind    0000000000000000 .orc_unwind
0000000000000000 l    d  .note.gnu.property     0000000000000000 .note.gnu.property
0000000000000000 l    d  .note.gnu.build-id     0000000000000000 .note.gnu.build-id
0000000000000000 l    d  .note.Linux    0000000000000000 .note.Linux
0000000000000000 l    d  .data  0000000000000000 .data
0000000000000000 l    d  .gnu.linkonce.this_module      0000000000000000 .gnu.linkonce.this_module
0000000000000000 l    d  .bss   0000000000000000 .bss
0000000000000000 l    d  .debug_info    0000000000000000 .debug_info
0000000000000000 l    d  .debug_abbrev  0000000000000000 .debug_abbrev
0000000000000000 l    d  .debug_aranges 0000000000000000 .debug_aranges
0000000000000000 l    d  .debug_rnglists        0000000000000000 .debug_rnglists
0000000000000000 l    d  .debug_line    0000000000000000 .debug_line
0000000000000000 l    d  .debug_str     0000000000000000 .debug_str
0000000000000000 l    d  .debug_line_str        0000000000000000 .debug_line_str
0000000000000000 l    d  .comment       0000000000000000 .comment
0000000000000000 l    d  .note.GNU-stack        0000000000000000 .note.GNU-stack
0000000000000000 l    d  .debug_frame   0000000000000000 .debug_frame
0000000000000000 l    df *ABS*  0000000000000000 hello.c
0000000000000000 l     F .init.text     0000000000000012 lk_hello
0000000000000000 l     F .exit.text     000000000000000e lk_exit
0000000000000000 l     O .modinfo       000000000000000c __UNIQUE_ID_license27
0000000000000000 l    df *ABS*  0000000000000000 hello.mod.c
0000000000000000 l     O .note.Linux    0000000000000018 _note_6
0000000000000010 l     O .modinfo       0000000000000022 __UNIQUE_ID_vermagic26
0000000000000032 l     O .modinfo       000000000000000b __UNIQUE_ID_name27
000000000000003d l     O .modinfo       000000000000000c __UNIQUE_ID_retpoline28
0000000000000050 l     O .modinfo       0000000000000009 __module_depends
0000000000000000 g     O .gnu.linkonce.this_module      0000000000000340 __this_module
0000000000000000 g     F .exit.text     000000000000000e cleanup_module
0000000000000000 g     F .init.text     0000000000000012 init_module
0000000000000000         *UND*  0000000000000000 printk
```


arm-linux-objdump
参数选项：
-a
--archive-headers 
显示档案库的成员信息,类似ls -l将lib*.a的信息列出。 

-C 
--demangle 
将底层的符号名解码成用户级名字，除了去掉所开头的下划线之外，还使得C++函数名以可理解的方式显示出来。 

-g
--debugging 
显示调试信息。企图解析保存在文件中的调试信息并以C语言的语法显示出来。仅仅支持某些类型的调试信息。有些其他的格式被readelf -w支持。 

-e 
--debugging-tags 
类似-g选项，但是生成的信息是和ctags工具相兼容的格式。 

-d
--disassemble 
从objfile中反汇编那些特定指令机器码的section。 

-D 
--disassemble-all 
与 -d 类似，但反汇编所有section. 

--prefix-addresses 
反汇编的时候，显示每一行的完整地址。这是一种比较老的反汇编格式。 

-EB 
-EL 
--endian={big|little} 
指定目标文件的小端。这个项将影响反汇编出来的指令。在反汇编的文件没描述小端信息的时候用。例如S-records. 

-f 
--file-headers 
显示objfile中每个文件的整体头部摘要信息。 

-h 
--section-headers 
--headers 
显示目标文件各个section的头部摘要信息。 

-H 
--help 
简短的帮助信息。 

-i 
--info 
显示对于 -b 或者 -m 选项可用的架构和目标格式列表。 

-j name
--section=name 
仅仅显示指定名称为name的section的信息 

-l
--line-numbers 
用文件名和行号标注相应的目标代码，仅仅和-d、-D或者-r一起使用使用-ld和使用-d的区别不是很大，在源码级调试的时候有用，要求编译时使用了-g之类的调试编译选项。 

-m machine 
--architecture=machine 
指定反汇编目标文件时使用的架构，当待反汇编文件本身没描述架构信息的时候(比如S-records)，这个选项很有用。可以用-i选项列出这里能够指定的架构. 

-r
--reloc 
显示文件的重定位入口。如果和-d或者-D一起使用，重定位部分以反汇编后的格式显示出来。 

-R
--dynamic-reloc 
显示文件的动态重定位入口，仅仅对于动态目标文件意义，比如某些共享库。 

-s 
--full-contents 
显示指定section的完整内容。默认所有的非空section都会被显示。 

-S 
--source 
尽可能反汇编出源代码，尤其当编译的时候指定了-g这种调试参数时，效果比较明显。隐含了-d参数。 

--show-raw-insn 
反汇编的时候，显示每条汇编指令对应的机器码，如不指定--prefix-addresses，这将是缺省选项。 

--no-show-raw-insn 
反汇编时，不显示汇编指令的机器码，如不指定--prefix-addresses，这将是缺省选项。 

--start-address=address 
从指定地址开始显示数据，该选项影响-d、-r和-s选项的输出。 

--stop-address=address 
显示数据直到指定地址为止，该项影响-d、-r和-s选项的输出。 

-t 
--syms 
显示文件的符号表入口。类似于nm -s提供的信息 

-T 
--dynamic-syms 
显示文件的动态符号表入口，仅仅对动态目标文件意义，比如某些共享库。它显示的信息类似于 nm -D|--dynamic 显示的信息。 

-V 
--version 
版本信息 

-x
--all-headers 
显示所可用的头信息，包括符号表、重定位入口。-x 等价于-a -f -h -r -t 同时指定。 

-z 
--disassemble-zeroes 
一般反汇编输出将省略大块的零，该选项使得这些零块也被反汇编。 

@file 
可以将选项集中到一个文件中，然后使用这个@file选项载入。
