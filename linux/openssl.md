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

## install
```shell
<!-- 1.1 版本 -->
paru -S openssl-1.1

paru -S openssl
```

## 常用命令
- 生成私钥
```shell
openssl genrsa -out private.pem 2048
```

- 生成公钥
```shell
openssl rsa -in private.pem -outform PEM -pubout -out public.pem
```


- 支持的加密算法
```shell
openssl ciphers -v
```

- pem to crt
```shell
openssl x509 -outform der -in cacert.pem -out client.crt
```

- 生成证书 
```shell
openssl req -new -x509 -days 365 -nodes -out secret.pem -keyout secret.key 
openssl pkcs8 -took8 -inform PEM -outform PEM -in secret.key -out outfile.pem 
```

- 签名文件 
```shell
openssl dgst -sha256 -sign secret.key -out signature.sha256 file.txt
```

- 验证签名 
```shell
openssl dgst -sha256 -verify secret.pem -signature signature.sha256 file.txt
```

- 加密文件 
```shell
openssl enc -aes-256-cbc -in file.txt -out file.enc -k secret
```

- 解密文件 
```shell
openssl enc -d -aes-256-cbc -in file.enc -out file.txt -k secret
```

- 导出证书 
```shell
openssl x509 -in cert.pem -outform der -out cert.der
```

- 导入证书 
```shell
openssl x509 -inform der -in cert.der -outform pem -out cert.pem
```

- 导出私钥 
```shell
openssl rsa -in secret.key -outform der -out secret.der
```

- 导入私钥 
```shell
openssl rsa -inform der -in secret.der -outform pem -out secret.key
```

- 导出公钥 
```shell
openssl rsa -in secret.key -outform der -pubout -out public.der
```

- 导入公钥 
```shell
openssl rsa -inform der -in public.der -outform pem -out public.pem
```

- 导出密钥对 
```shell
openssl pkcs12 -export -inkey secret.key -in secret.pem -out secret.p12
```

- 导入密钥对 
```shell
openssl pkcs12 -in secret.p12 -out secret.pem -nocerts -nodes
```

- 导出证书链 
```shell
cat secret.pem intermediate.pem > chain.pem
```

- 导入证书链 
```shell
cat chain.pem intermediate.pem > secret.pem
```

- 导出证书请求 
```shell
openssl req -in csr.pem -outform der -out csr.der
```
