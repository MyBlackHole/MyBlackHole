# openssl

- 生成证书 
```shell
openssl req -new -x509 -days 365 -nodes -out secret.pem -keyout secret.key 
openssl pkcs8 -took8 -inform PEM -outform PEM -in secret.key -out outfile.pem 
```

