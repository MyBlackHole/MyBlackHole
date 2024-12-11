# disassemble

```shell
❯ gdb open.o
GNU gdb (GDB) 15.2
Copyright (C) 2024 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-pc-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from open.o...
(gdb) disa
disable      disassemble
(gdb) disassemble filp_close
Dump of assembler code for function filp_close:
Address range 0x70 to 0xcb:
   0x0000000000000070 <+0>:     push   %r12
   0x0000000000000072 <+2>:     push   %rbp
   0x0000000000000073 <+3>:     push   %rbx
   0x0000000000000074 <+4>:     mov    0x38(%rdi),%rax
   0x0000000000000078 <+8>:     test   %rax,%rax
   0x000000000000007b <+11>:    je     0x81 <filp_close+17>
   0x0000000000000081 <+17>:    mov    0x28(%rdi),%rax
   0x0000000000000085 <+21>:    mov    %rdi,%rbx
   0x0000000000000088 <+24>:    mov    %rsi,%rbp
   0x000000000000008b <+27>:    xor    %r12d,%r12d
   0x000000000000008e <+30>:    mov    0x70(%rax),%rax
   0x0000000000000092 <+34>:    test   %rax,%rax
   0x0000000000000095 <+37>:    je     0x9f <filp_close+47>
   0x0000000000000097 <+39>:    call   0x9c <filp_close+44>
   0x000000000000009c <+44>:    mov    %eax,%r12d
   0x000000000000009f <+47>:    testb  $0x40,0x45(%rbx)
   0x00000000000000a3 <+51>:    jne    0xbb <filp_close+75>
   0x00000000000000a5 <+53>:    mov    %rbp,%rsi
   0x00000000000000a8 <+56>:    mov    %rbx,%rdi
   0x00000000000000ab <+59>:    call   0xb0 <filp_close+64>
   0x00000000000000b0 <+64>:    mov    %rbp,%rsi
   0x00000000000000b3 <+67>:    mov    %rbx,%rdi
   0x00000000000000b6 <+70>:    call   0xbb <filp_close+75>
   0x00000000000000bb <+75>:    mov    %rbx,%rdi
   0x00000000000000be <+78>:    call   0xc3 <filp_close+83>
   0x00000000000000c3 <+83>:    mov    %r12d,%eax
   0x00000000000000c6 <+86>:    pop    %rbx
   0x00000000000000c7 <+87>:    pop    %rbp
   0x00000000000000c8 <+88>:    pop    %r12
   0x00000000000000ca <+90>:    ret
Address range 0x26c0 to 0x26d4:
   0x00000000000026c0 <+9808>:  mov    $0x0,%rdi
   0x00000000000026c7 <+9815>:  xor    %r12d,%r12d
   0x00000000000026ca <+9818>:  call   0x26cf <filp_close+9823>
   0x00000000000026cf <+9823>:  jmp    0x26d4 <do_dentry_open.cold>
End of assembler dump.

```
