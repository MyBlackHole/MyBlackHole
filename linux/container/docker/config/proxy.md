# proxy

- 代理
```shell
nvim /etc/default/docker 
export http_proxy="http://127.0.0.1:1081/" 

# 或

lvim .docker/config.json
{
    "proxies": {
        "default": {
            "httpProxy": "http://127.0.0.1:1080",
            "httpsProxy": "http://127.0.0.1:1080"
            "noProxy": "*.test.example.com,.example2.com" 
        }
    }
}
```

- docker run

|环境变量|docker run 示例|
|---|---|
|HTTP_PROXY|--env HTTP_PROXY="http://proxy.example.com:8080/"|
|HTTPS_PROXY|--env HTTPS_PROXY="http://proxy.example.com:8080/"|
|NO_PROXY|--env NO_PROXY="localhost,127.0.0.1,.example.com"|

- etc
```shell
<!-- 当前 user -->
mkdir -p ~/.config/systemd/user/docker.service.d 
~/.config/systemd/user/docker.service.d/http-proxy.conf 
[Service] 
Environment="HTTP_PROXY=192.168.5.233:1081" 
Environment="HTTPS_PROXY=192.168.5.233:1081" 

刷新systemctl配置 
sudo systemctl daemon-reload 

验证环境变量 
systemctl show —property=Environment docker 

重启docker 


<!-- 全局 regular -->
sudo mkdir –p /etc/systemd/system/docker.service.d 

/etc/systemd/system/docker.service.d/http-proxy.conf 

[Service] 
Environment="HTTP_PROXY=http://proxy.example.com:80" 
Environment="HTTPS_PROXY=https://proxy.example.com:443" 
sudo systemctl daemon-reload
sudo systemctl restart docker
```

- --build-arg
```shell
docker build . \
    --build-arg "HTTP_PROXY=http://127.0.0.1:1080" \
    --build-arg "HTTPS_PROXY=http://127.0.0.1:1080" \
    --build-arg "NO_PROXY=localhost,127.0.0.1,.example.com" \
    -t your/image:tag
```


- Dockerfile

|环境变量|Dockerfile 示例|
|---|---|
|HTTP_PROXY|ENV HTTP_PROXY="http://proxy.example.com:8080/"|
|HTTPS_PROXY|ENV HTTPS_PROXY="http://proxy.example.com:8080/"|
|NO_PROXY|ENV NO_PROXY="localhost,127.0.0.1,.example.com"|

- docker 国内镜像地址
```shell
cat << EOF > /etc/docker/daemon.json 
{ 
  "registry-mirrors": ["http://hub-mirror.c.163.com"], 
  "insecure-registries": ["10.7.92.101:5000"] 
} 
EOF 

#执行如下命令重启docker 
systemctl restart docker 
```

