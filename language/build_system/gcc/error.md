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