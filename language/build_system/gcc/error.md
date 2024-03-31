# error

- 错误：只允许在 C99 模式下使用‘for’循环初始化声明
```shell
gcc xxx.c --std=c99
```

- 错误： ‘enum fsconfig_command’多次定义
https://github.com/gcc-mirror/gcc/commit/2701442d0cf6292f6624443c15813d6d1a3562fe

- sanitizer_common/sanitizer_internal_defs.h:254:72: error: size of array ‘assertion_failed__1141’ is negative
```shell
configure 加入
--disable-libsanitizer

```

- temporary of non-literal type ‘google::protobuf::Any’ in a constant expression
```shell
升级 gcc >= 7.4
```

- configure: error: C++ preprocessor "/lib/cpp" fails sanity check
```shell
yum install gcc-c++
yum install glibc-headers (不一定要)
```

- cc1plus没有找到或目录
```shell
yum install gcc-c++
```
