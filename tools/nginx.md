# nginx

- 隐藏版本号
```shell
lvim nginx.conf

server {
    server_tokens off; 
    ......
}
```

- podman
```shell
podman run -it --rm -d -p 8080:80 --name web nginx
```
