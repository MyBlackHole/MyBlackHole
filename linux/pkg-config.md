# pkg-config
获取 连接库 的标记  
```shell
pkg-config --cflags fuse
-D_FILE_OFFSET_BITS=64 -I/usr/include/fuse

pkg-config fuse3 --cflags --libs
```
