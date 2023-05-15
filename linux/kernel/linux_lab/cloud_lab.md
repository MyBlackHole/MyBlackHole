# cloud lab

[细节](https://gitee.com/tinylab/linux-lab)
```shell
# 使用非 root 用户
git clone https://gitee.com/tinylab/cloud-lab.git
cd cloud-lab/

# 启动 linux lab
tools/docker/run linux-lab

# bash 登陆
tools/docker/bash

# web 登陆
tools/docker/webvnc

# 其他
tools/docker/vnc
tools/docker/ssh
tools/docker/webssh

# 选择登陆方式
tools/docker/login list  # 列出并选择，并且记住
tools/docker/login vnc    # 直接选择一种并记住
```

登录方法	描述	缺省用户	登录所在地
bash	docker bash	Ubuntu	本地主机
ssh	普通 ssh	Ubuntu	本地主机
vnc	普通 桌面	Ubuntu	本地主机+VNC client
webvnc	web 桌面	Ubuntu	本地主机或互联网
webssh	web ssh	Ubuntu	本地主机或互联网
