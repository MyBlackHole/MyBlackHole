# config

- 普通用户 attach
```shell
sudo sysctl kernel.yama.ptrace_scope=0
```

- 修改编译路径
```shell
<!-- 修改 root1 目录为 root 目录 -->
set substitute-path /root1/ /root/

show substitute-path
```
