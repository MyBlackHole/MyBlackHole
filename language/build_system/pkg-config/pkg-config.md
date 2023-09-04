# pkg-config

- 依赖查找
```shell
pkg-config libxxx --cflags
```

- 设置路径配置
```shell
export PKG_CONFIG_PATH="/usr/local/opt/icu4c/lib/pkgconfig:${PKG_CONFIG_PATH}"
export PKG_CONFIG_PATH="/usr/local/opt/jpeg-turbo/lib/pkgconfig:${PKG_CONFIG_PATH}"
```

- 模板 pc.in
```shell
<!-- 其中 @variable@ 是变量，会被 configure 替换。接下来就是在 configure.ac 文件中通过 AC_CONFIG_FILES 添加 pc 文件，接下来在库被安装的时候，通过 configure 正确设置变量后，就可以动态生成 pc 文件和库的其他文件一起安装在合适的位置了 -->

prefix=@prefix@
exec_prefix=@exec_prefix@
libdir=@libdir@
includedir=@includedir@

Name: libwebp
Description: Library for the WebP graphics format
Version: @PACKAGE_VERSION@
Cflags: -I${includedir}
Libs: -L${libdir} -lwebp
Libs.private: -lm @PTHREAD_CFLAGS@ @PTHREAD_LIBS@
```
