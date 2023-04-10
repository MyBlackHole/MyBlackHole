# 异常处理集

- 连接权限不够
```shell
# 将当前用户添加到 docker 组(重启生效)
sudo usermod -aG docker $USER
```

- E: dpkg was interrupted, you must manually run 'dpkg --configure -a' to correct the problem.  
```shell
rm –r /var/lib/dpkg/updates/* 
```

- 访问第三方ssl的服务异常
```shell
安装ca-certificates
```

- container b5257bd0ae8d is using its referenced image cd7087199d1b 
```shell
镜像还有其他容器引用 
删除即可 
```

- denied: requested access to the resource is denied 
```shell
先tag（标签）： 加上blackahole/，再push（不要加sudo)
```

- Dokcer配置文件权限问题
```shell
sudo chown "$USER":"$USER" /home/"$USER"/.docker -R 
sudo chmod g+rwx "/home/$USER/.docker" -R 
```

- Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running? 
```shell
# 先关闭原来的管理员WSL界面，重新开启一个 
# 首先需要进入管理员的WSL，然后直接进入root用户，直接在root用户启动docker，就可以了 
service docker start 
```

- 时区
```shell
RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo 'Asia/Shanghai' > /etc/timezone 
```

- Docker容器做端口映射报错 docker: Error response from daemon: driver failed programming external connectivity  
```shell
# 重启 docker 服务
```
