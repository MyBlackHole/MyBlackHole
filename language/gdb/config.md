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

set substitute-path / /root/fs-backup/fsdeamon/

unset substitute-path /root1/
```

- 修改 debug 路径
```shell
show debug-file-directory
set debug-file-directory /root/fs-backup/fsdeamon/


unset debug-file-directory /root/fs-backup/fsdeamon/
```

- 设置源码路径
```shell
<!-- 最好直接进入源码目录，然后执行 gdb 命令(前提是编译时基于源码目录编译) -->

directory fsdeamon/

<!-- gdb -q a.out -d /search/code/some -->
```
