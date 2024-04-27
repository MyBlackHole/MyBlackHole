# kernel.ld

链接脚本文件，用于链接内核对象文件。

## 例子
```shell
{
    . = 0x10000;
    .text : { *(.text) }
    . = 0x8000000;
    .data : { *(.data) }
    .bss : { *(.bss) }
}

SECTIONS是LS语法中的关键command，它用来描述输出文件的内存布局。
    例如上例中就含 text/data/bss 三个部分
    (实际上text/data/bss才是段，但是 SECTIONS 这个词在LS中是一个 command，希望各位看官要明白)

.=0x10000: 其中的.非常关键，它代表 location counter(LC)。
    意思是 .text 段的开始设置在 0x10000 处。
    这个 LC 应该指的是 LMA，但大多数情况下 VMA=LMA。

.text:{*(.text)}: 这个表示输出文件的.text段内容由所有输入文件(*)的.text段组成。
    组成顺序就是ld命令中输入文件的顺序，例如1.obj,2.obj......

.=0x800000000:
    如果没有这个赋值的，那么 LC 应该等于 0x10000+sizeof(text段)，
    即 LC 如果不强制指定的话，它默认就是上一次的 LC + 中间 section 的长度。
    还好，这里强制指定 LC=0X800000000. 表明后面的.data 段的开始位于这个地址。

.data 和后面的 .bss 表示分别有输入文件的 .data 和 .bss 段构成。
```
