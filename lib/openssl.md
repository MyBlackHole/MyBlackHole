# openssl
加解密库

## build
```shell
OpenSSL_1_1_1
./config --prefix=/media/black/Data/lib/openssl/openssl_1_1_1 --openssldir=/media/black/Data/lib/openssl/openssl_1_1_1_dir
--prefix 指定安装路径
--openssldir 配置参数路径
perl configdata.pm --dump
make
make install
```
