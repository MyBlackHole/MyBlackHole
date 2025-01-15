- 安装环境
```shell
tools/docker/choose
```
- 删除环境
```shell
tools/docker/rm linux-lab
```

- 运行环境
```shell
tools/docker/run linux-lab

INFO: Available login methods:

     bash vnc ssh webssh webvnc

INFO: Switch to another method:

     tools/docker/login LOGIN_METHOD

INFO: Running '/media/black/Data/Documents/linux_debug/cloud-lab/tools/docker/ssh'

INFO: Wait for ssh service launching ...
ERR: No web browser found, are you running me in cloud? please open the 'Normal' url yourself.
//var/log/sshd.log



access control disabled, clients can connect from any host


Warning: Permanently added '172.27.189.130' (ED25519) to the list of known hosts.
/usr/bin/xauth:  file /home/ubuntu/.Xauthority does not exist


# 开始 debug
make debug

使用 root 帐号登录，不需要输入密码（密码为空），只需要输入 root 然后输入回车即可
```
