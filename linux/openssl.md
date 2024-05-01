# openssl

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
