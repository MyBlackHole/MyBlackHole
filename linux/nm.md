# nm
列举 [文件] 中的符号 (默认为 a.out)。
注意: -g 编译

## 格式
nm [option(s)] [file(s)]

### options
A 在每个符号信息的前面打印所在对象文件名称；
C 输出demangle过了的符号名称；
D 打印动态符号；
l 使用对象文件中的调试信息打印出所在源文件及行号；
n 按照地址/符号值来排序；
u 打印出那些未定义的符号；



## 例子
- 打印动态符号
```shell
nm -D liba.so
                 w __cxa_finalize
00000000000010f9 T func_versioning@@VER_2.0
                 w __gmon_start__
                 w _ITM_deregisterTMCloneTable
                 w _ITM_registerTMCloneTable
0000000000000000 A VER_2.0
```

```shell
nm ./out/obj/pthread_learn/pthread_getname_np_test
000000000000038c r __abi_tag
                 U atoi@GLIBC_2.2.5
0000000000004010 B __bss_start
0000000000004010 b completed.0
                 w __cxa_finalize@GLIBC_2.2.5
0000000000004000 D __data_start
0000000000004000 W data_start
00000000000011d0 t deregister_tm_clones
0000000000001240 t __do_global_dtors_aux
0000000000003d70 d __do_global_dtors_aux_fini_array_entry
0000000000004008 D __dso_handle
0000000000003d78 d _DYNAMIC
0000000000004010 D _edata
0000000000004018 B _end
                 U __errno_location@GLIBC_2.2.5
                 U exit@GLIBC_2.2.5
00000000000014a4 T _fini
0000000000001280 t frame_dummy
0000000000003d68 d __frame_dummy_init_array_entry
00000000000021b4 r __FRAME_END__
0000000000003f68 d _GLOBAL_OFFSET_TABLE_
                 w __gmon_start__
00000000000020b4 r __GNU_EH_FRAME_HDR
0000000000001000 T _init
0000000000002000 R _IO_stdin_used
                 w _ITM_deregisterTMCloneTable
                 w _ITM_registerTMCloneTable
                 U __libc_start_main@GLIBC_2.34
00000000000012aa T main
                 U perror@GLIBC_2.2.5
                 U printf@GLIBC_2.2.5
                 U pthread_create@GLIBC_2.34
                 U pthread_getname_np@GLIBC_2.34
                 U pthread_join@GLIBC_2.34
                 U pthread_setname_np@GLIBC_2.34
                 U puts@GLIBC_2.2.5
0000000000001200 t register_tm_clones
                 U sleep@GLIBC_2.2.5
00000000000011a0 T _start
0000000000001289 t threadfunc
0000000000004010 D __TMC_END__
```
