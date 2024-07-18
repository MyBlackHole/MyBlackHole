# redis

- podman
```shell
<!-- 拉取镜像同时运行 -->
podman run -p 6379:6379 --name redis -d redis

<!-- 设置密码 -->
podman run -d -p 6379:6379 --name aio-redis redis --requirepass "88888888888888"
```

- 物理机
```shell
wget http://download.redis.io/releases/redis-5.0.3.tar.gz 
tar xzf redis-5.0.3.tar.gz 
sudo mv ./redis-5.0.3 /usr/local/redis/ 
cd /usr/local/redis/ 
sudo make (生成二进制文件) 
sudo make install (默认安装在/usr/local/bin/) 

redis-server # redis 服务器 
redis-cli redis # 命令行客户端 
redis-benchmark # redis 性能测试工具 
redis-check-aof # AOF 文件修复工具 
redis-check-rdb # RDB 文件检索工具 

# win
redis-server --service-install redis.windows.conf --service-name redis  #守护启动 
```

### 线程模型
![[imgs/GetImage.png]]
