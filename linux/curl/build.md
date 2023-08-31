# build
```shell
<!-- ldap: Lightweight Directory Access Protocol，「轻量目录访问协议」 -->
./configure CFLAGS='-g -O0' --prefix=/media/black/Data/lib/curl/7_35_0 --without-ssl --disable-ldap

<!-- 静态构建 -->
<!-- 不启用 ldap -->
./configure CFLAGS='-g -O0' --prefix=/media/black/Data/lib/curl/7_35_0 --enable-shared=no --enable-static=yes --disable-ldap
```
