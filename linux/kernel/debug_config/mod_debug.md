# mod_debug

给内核模块打断点需要使用 hbreak 打硬件断点
CONFIG_DEBUG_RODATA=n
CONFIG_DEBUG_RODATA_TEST=n
CONFIG_DEBUG_SET_MODULE_RONX=n
然后参照上面的方法加载调试符号即可，可以直接使用 break 来设置断点，不需要用硬件断点


- 快速获取模块地址
```shell
#!/bin/bash
cd /sys/module/$1/sections
echo -n add-symbol-file $2 `/bin/cat .text`
for section in .[a-z]* *; do
    if [ $section != ".text" ]; then
echo " \\"
echo -n " -s" $section `/bin/cat $section`
    fi
done
echo
```


- 调试再入函数 (没起作用)
```shell

objdump -t hello.ko
0000000000000000 l     F .init.text     0000000000000012 lk_hello

break do_init_module
c

p /x mod.sect_attrs.attrs[1].address
$1 = 0xffffffffa0000000

print mod->sect_attrs->attrs[1]->name 
print mod->sect_attrs->attrs[7]->name 
print mod->sect_attrs->attrs[9]->name 
print /x mod->sect_attrs->attrs[1]->address 
print /x mod->sect_attrs->attrs[7]->address 
print /x mod->sect_attrs->attrs[9]->address

<!-- add-symbol-file /run/media/black/Data/Documents/c/linux_module_learn/hello/hello.ko <text addr> -s .data <data addr> -s .bss <bss addr> -->
add-symbol-file /run/media/black/Data/Documents/c/linux_module_learn/hello/hello.ko 0xffffffffa0000000

break lk_hello

<!-- 给地址下断点 -->
break *0xffffffffa0000012


# cat /proc/modules
hello 16384 0 - Live 0xffffffffa0000000 (O)



(gdb) p  mod.sect_attrs.attrs[0].battr.attr.name
$5 = 0xffff88813afd2170 ".init.text"


(gdb) p  mod.sect_attrs.attrs[1].battr.attr.name
$7 = 0xffff88813afd2160 ".exit.text"


<!-- 如果断点设置在module中的某个函数，执行代码断点没生效，解决方法： -->
<!-- 1）kernel编译关闭：Randomize the address of the kernel image (KASLR) -->
<!-- 2）kernel编译打开： -->
<!--   [*]    Compile the kernel with debug info -->
<!--   [*]    Provide GDB scripts for kernel debugging -->
<!-- 3）断点函数是inline类型，需去掉inline属性 -->
<!-- 4）断点函数被优化，需加 __attribute__((optimize("O0")));声明不要优化，如： -->
<!-- int upload_to_server(char *server_dir, char *local_file) __attribute__((optimize("O0"))); -->

可以更改/etc/grub2.conf 添加 nokasrl 命令参数。
然后`grub2-mkconfig -o /boot/grub2/grub.cfg`

Processor type and features -> 
[ ] Build a relocatable kernel
取消后 Build a relocatable kernel 的子项 Randomize the address of the kernel image (KASLR) 也会一并被取消

```

- 载入后调试模块
```shell
insmod fsbackup.ko


cat /sys/module/fsbackup/sections/.text
cat /sys/module/fsbackup/sections/.data
cat /sys/module/fsbackup/sections/.bss
0xffffffffa0000000
0xffffffffa000d000
0xffffffffa000dbc0

<!-- host gdb -->
lvim ~/.gdbinit
add-auto-load-safe-path /root/code/linux-4.18.2/scripts/gdb/vmlinux-gdb.py
target remote:1234

gdb ./vmlinux

add-symbol-file ./fsbackup.ko 0xffffffffa0000000 -s .data 0xffffffffa000d000 -s .bss 0xffffffffa000dbc0

# cat /sys/module/fsbackup/sections/.text
0xffffffffa000d000
# cat /sys/module/fsbackup/sections/.data
0xffffffffa0015000
# cat /sys/module/fsbackup/sections/.bss
0xffffffffa0015540

add-symbol-file /home/black/Public/aio/aio-tools/fsbackup_kernel_4.x/fsbackup.ko 0xffffffffa0000000 -s .data 0xffffffffa0008000 -s .bss 0xffffffffa0008540


# cat /sys/module/fsbackup/sections/.text
0xffffffffa0000000
# cat /sys/module/fsbackup/sections/.data
0xffffffffa0008000
# cat /sys/module/fsbackup/sections/.bss
0xffffffffa0008540


break fsbackup_init
```

- 通过 objdump 分析模块符号
```shell
<!-- 主要符号 -->
❯ objdump --section-headers fsbackup.ko | grep .text
  0 .text         000079c3  0000000000000000  0000000000000000  00000040  2**0
  1 .init.text    00000540  0000000000000000  0000000000000000  00007a03  2**0
  2 .exit.text    00000201  0000000000000000  0000000000000000  00007f43  2**0

❯ objdump --section-headers fsbackup.ko | grep .data
  4 .rodata       00000468  0000000000000000  0000000000000000  00008160  2**5
  5 .rodata.str1.1 000006af  0000000000000000  0000000000000000  000085c8  2**0
 10 .rodata.str1.8 0000099c  0000000000000000  0000000000000000  00009758  2**3
 12 .data         00000200  0000000000000000  0000000000000000  0000a100  2**5


❯ objdump --section-headers fsbackup.ko | grep .bss
 18 .bss          00000214  0000000000000000  0000000000000000  0000a6c0  2**5

<!-- 查询加载地址 -->
cat /proc/modules
fsbackup 53248 0 - Live 0xffffffffa0000000 (O)



<!-- Add-symbol-file *.ko text_addr –s .section0 section_addr0 -s .section1 section_addr1 -->
<!-- Text_addr = install_addr + file_off -->
<!-- .section_addr0 = install_addr + file_off -->
<!-- 如：.text 段 -->
<!-- Section_addr = install_addr + file_off = 0xffffffffa0000000 + 0x40 = 0xffffffffa0000040 -->
<!-- 如：.data 段 -->
<!-- Section_addr = install_addr + file_off = 0xffffffffa0000000 + 0xa100 = 0xffffffffa000a100 -->
<!-- 如：.bss 段 -->
<!-- Section_addr = install_addr + file_off = 0xffffffffa0000000 + 0xa6c0 = 0xffffffffa000a6c0 -->
add-symbol-file ./drivers/mailbox/module_test.ko 0xffffffffa0000040 -s .data 0xffffffffa000a100 -s .bss 0xffffffffa000a6c0




objdump --section-headers fsbackup.ko
fsbackup.ko:     file format elf64-x86-64

Sections:
Idx Name          Size      VMA               LMA               File off  Algn
  0 .text         000079c3  0000000000000000  0000000000000000  00000040  2**0
                  CONTENTS, ALLOC, LOAD, RELOC, READONLY, CODE
  1 .init.text    00000540  0000000000000000  0000000000000000  00007a03  2**0
                  CONTENTS, ALLOC, LOAD, RELOC, READONLY, CODE
  2 .exit.text    00000201  0000000000000000  0000000000000000  00007f43  2**0
                  CONTENTS, ALLOC, LOAD, RELOC, READONLY, CODE
  3 __ksymtab     00000008  0000000000000000  0000000000000000  00008148  2**3
                  CONTENTS, ALLOC, LOAD, RELOC, READONLY, DATA
  4 .rodata       00000468  0000000000000000  0000000000000000  00008160  2**5
                  CONTENTS, ALLOC, LOAD, RELOC, READONLY, DATA
  5 .rodata.str1.1 000006af  0000000000000000  0000000000000000  000085c8  2**0
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
  6 .modinfo      00000059  0000000000000000  0000000000000000  00008c78  2**3
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
  7 .orc_unwind_ip 00000414  0000000000000000  0000000000000000  00008cd1  2**0
                  CONTENTS, ALLOC, LOAD, RELOC, READONLY, DATA
  8 .orc_unwind   0000061e  0000000000000000  0000000000000000  000090e5  2**0
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
  9 .smp_locks    00000054  0000000000000000  0000000000000000  00009704  2**2
                  CONTENTS, ALLOC, LOAD, RELOC, READONLY, DATA
 10 .rodata.str1.8 0000099c  0000000000000000  0000000000000000  00009758  2**3
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
 11 __ksymtab_strings 00000009  0000000000000000  0000000000000000  0000a0f4  2**0
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
 12 .data         00000200  0000000000000000  0000000000000000  0000a100  2**5
                  CONTENTS, ALLOC, LOAD, RELOC, DATA
 13 __bug_table   0000000c  0000000000000000  0000000000000000  0000a300  2**0
                  CONTENTS, ALLOC, LOAD, RELOC, DATA
 14 .note.gnu.property 00000030  0000000000000000  0000000000000000  0000a310  2**3
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
 15 .note.gnu.build-id 00000024  0000000000000000  0000000000000000  0000a340  2**2
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
 16 .note.Linux   00000018  0000000000000000  0000000000000000  0000a364  2**2
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
 17 .gnu.linkonce.this_module 00000340  0000000000000000  0000000000000000  0000a380  2**6
                  CONTENTS, ALLOC, LOAD, RELOC, DATA, LINK_ONCE_DISCARD
 18 .bss          00000214  0000000000000000  0000000000000000  0000a6c0  2**5
                  ALLOC
 19 .comment      000000a8  0000000000000000  0000000000000000  0000a6c0  2**0
                  CONTENTS, READONLY
 20 .note.GNU-stack 00000000  0000000000000000  0000000000000000  0000a768  2**0
                  CONTENTS, READONLY
 21 .debug_aranges 00000140  0000000000000000  0000000000000000  0000a768  2**0
                  CONTENTS, RELOC, READONLY, DEBUGGING, OCTETS
 22 .debug_info   00048baa  0000000000000000  0000000000000000  0000a8a8  2**0
                  CONTENTS, RELOC, READONLY, DEBUGGING, OCTETS
 23 .debug_abbrev 00002947  0000000000000000  0000000000000000  00053452  2**0
                  CONTENTS, READONLY, DEBUGGING, OCTETS
 24 .debug_line   0000383d  0000000000000000  0000000000000000  00055d99  2**0
                  CONTENTS, RELOC, READONLY, DEBUGGING, OCTETS
 25 .debug_frame  00000ea8  0000000000000000  0000000000000000  000595d8  2**3
                  CONTENTS, RELOC, READONLY, DEBUGGING, OCTETS
 26 .debug_str    0002da5f  0000000000000000  0000000000000000  0005a480  2**0
                  CONTENTS, READONLY, DEBUGGING, OCTETS
 27 .debug_line_str 00002cc8  0000000000000000  0000000000000000  00087edf  2**0
                  CONTENTS, READONLY, DEBUGGING, OCTETS
 28 .debug_rnglists 0000004f  0000000000000000  0000000000000000  0008aba7  2**0
                  CONTENTS, RELOC, READONLY, DEBUGGING, OCTETS
```
