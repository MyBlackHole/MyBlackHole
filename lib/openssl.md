# openssl
加解密库

## build
```shell
OpenSSL_1_1_1 (有些版本默认静态编译 shared, 而不是 --enable-shared)
./config --prefix=/media/black/Data/lib/openssl/openssl_1_1_1 --openssldir=/media/black/Data/lib/openssl/openssl_1_1_1_dir
--prefix 指定安装路径
--openssldir 配置参数路径
perl configdata.pm --dump (有些版本不用)
make
make install
```

- 支持的加密算法
```shell
openssl ciphers -v
```

- pem to crt
```shell
openssl x509 -outform der -in cacert.pem -out client.crt
```
