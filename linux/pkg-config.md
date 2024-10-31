# pkg-config

- 获取连接库的标记
```shell
pkg-config --cflags fuse
-D_FILE_OFFSET_BITS=64 -I/usr/include/fuse

pkg-config fuse3 --cflags --libs
```

- 查询是否存在某个包
```shell
pkg-config --exists libxml-2.0
```

- 查询某个包 include 目录
```shell
pkg-config --variable=includedir libxml-2.0
```
