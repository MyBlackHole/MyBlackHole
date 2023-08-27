# lab

linux 学习环境
基于 docker

## install run
不能用 root 用户

### 下载环境
```shell
git clone https://gitee.com/tinylab/cloud-lab.git
cd cloud-lab/
```

### 运行登陆 linux lab

- 启动 Linux Lab 并根据控制台上打印的用户名和密码登录实验环境
```shell
tools/docker/run linux-lab
```

- 通过 Bash 直接登陆
```shell
tools/docker/bash
```

- 通过 Web 浏览器直接登录实验环境
```shell
tools/docker/webvnc
```

- 其他登录方式
```shell
tools/docker/vnc
tools/docker/ssh
tools/docker/webssh
```

- 选择某种登陆方式
```shell
tools/docker/login list  # 列出并选择，并且记住
tools/docker/login vnc    # 直接选择一种并记住
```


#### 登录方式汇总：
|   登录方法     |   描述             |  缺省用户        |  登录所在地          |
|----------------|--------------------|------------------|----------------------|
|   bash         | docker bash        |  Ubuntu          | 本地主机             |
|   ssh          | 普通 ssh           |  Ubuntu          | 本地主机             |
|   vnc          | 普通 桌面          |  Ubuntu          | 本地主机+VNC client  |
|   webvnc       | web 桌面           |  Ubuntu          | 本地主机或互联网     |
|   webssh       | web ssh            |  Ubuntu          | 本地主机或互联网     |
