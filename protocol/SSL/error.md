# error

- ssl.SSLError: [SSL: WRONG_VERSION_NUMBER] wrong version number (_ssl.c:1091) 
```shell
找到了requests和certifi证书版本问题，我原来安装这俩包都是最新的，按照

pip install requests==2.19.1

pip install certifi==2018.8.13

证书有问题

```
