# error 

- ISO C90 forbids mixed declarations and code in C
```c
{
    foo();
    int i = 0;
    bar();
}

// to
{
    int i = 0;
    foo();
    bar();
}
```

- 
undefined reference to 'inflate'
undefined reference to 'inflateEnd'
undefined reference to 'inflateInit_'
```shell
libz.a的链接顺序问题

target_link_libraries(oss_c_sdk_test m)
```

- fatal error: sys/acl.h: 没有那个文件或目录
```shell
sudo apt install libacl1-dev
```

- error while loading shared libraries: libnsl.so.1: cannot open shared object file: No such file or directory
```shell
yum install libnsl.x86_64
```
