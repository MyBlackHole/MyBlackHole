# kallsyms

里面包含了内核符号表，可以通过 `cat /proc/kallsyms` 命令查看。

ffffffff9b9bec10 t kallsyms_lookup_names xx

ffffffff9b9bec10:
- 符号地址

t:
- T：表示该符号是一个函数，可以被其他代码调用。
- t：表示该符号是一个局部函数，只能在当前文件中使用。
- D：表示该符号是一个全局变量，可以被其他代码访问和修改。
- d：表示该符号是一个局部变量，只能在当前文件中使用。
- R：表示该符号是一个只读变量，不能被修改。
- r：表示该符号是一个只读局部变量，只能在当前文件中使用。
- A：表示该符号是一个可读写的变量，可以被其他代码访问和修改。
- a：表示该符号是一个可读写的局部变量，只能在当前文件中使用。
- B：表示该符号是一个未初始化的全局变量，它的值在程序启动时被初始化为0。
- b：表示该符号是一个未初始化的局部变量，它的值在程序启动时被初始化为0。
- G：表示该符号是一个全局变量，但是它的值在程序运行时可能会被修改。
- C：表示该符号是一个常量，它的值在程序运行时不能被修改。
- W：表示该符号是一个弱符号，
- ?: 表示该符号的类型未知。

kallsyms_lookup_names:
符号名

xx:
驱动名

```shell
❯ cat /proc/kallsyms | grep kallsyms_lookup_names
0000000000000000 t __pfx_kallsyms_lookup_names
0000000000000000 t kallsyms_lookup_names

❯ sudo cat /proc/kallsyms | grep kallsyms_lookup_name
ffffffff9b990f40 T __pfx_module_kallsyms_lookup_name
ffffffff9b990f50 T module_kallsyms_lookup_name
ffffffff9b9bec00 t __pfx_kallsyms_lookup_names
ffffffff9b9bec10 t kallsyms_lookup_names
ffffffff9b9bf000 T __pfx_kallsyms_lookup_name
ffffffff9b9bf010 T kallsyms_lookup_name
ffffffff9ba7cae0 T __pfx_bpf_kallsyms_lookup_name
ffffffff9ba7caf0 T bpf_kallsyms_lookup_name
ffffffff9ca67a40 d bpf_kallsyms_lookup_name_proto

```
