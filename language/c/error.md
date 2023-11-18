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
