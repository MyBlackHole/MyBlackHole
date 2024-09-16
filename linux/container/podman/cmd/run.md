# run

运行容器

## 语法
```
podman run [options] IMAGE [COMMAND] [ARG...]
```

## 选项
| 选项 | 简写 | 功能 |
| --- |
| `--add-host` | `-h` | 添加主机名映射 |
| `--annotation` | `-a` | 添加注解 |
| `--cap-add` | `-c` | 添加 Linux 能力 |
| `--cap-drop` | `-d` | 去除 Linux 能力 |
| `--cgroup-parent` | `-g` | 设置父 cgroup |
| `--cidfile` | `-i` | 写入容器 ID 到文件 |
| `--conmon-pidfile` | `-p` | 设置 conmon PID 文件 |
| `--detach` | `-d` | 后台运行容器 |
| `--device` | `-D` | 增加设备映射 |
| `--dns` | `-n` | 设置 DNS 服务器 |
| `--dns-option` | `-o` | 设置 DNS 选项 |
| `--dns-search` | `-s` | 设置 DNS 搜索域 |
| `--entrypoint` | `-e` | 设置入口点 |
| `--env` | `-e` | 设置环境变量 |
| `--env-file` | `-E` | 从文件中读取环境变量 |
| `--expose` | `-p` | 开放端口 |
| `--group-add` | `-G` | 添加组 |
| `--healthcheck` | `-H` | 设置健康检查 |
| `--hostname` | `-h` | 设置主机名 |
| `--init` | `-i` | 使用 init 进程 |
| `--interactive` | `-i` | 交互式运行容器 |
| `--ip` | `-i` | 设置 IP 地址 |
| `--ipc` | `-i` | 设置 IPC 命名空间 |
| `--label` | `-l` | 添加标签 |
| `--label-file` | `-L` | 从文件中读取标签 |
| `--link` | `-l` | 添加链接 |
| `--log-driver` | `-l` | 设置日志驱动 |
| `--log-opt` | `-o` | 设置日志驱动选项 |
| `--mac-address` | `-m` | 设置 MAC 地址 |
| `--memory` | `-m` | 设置内存限制 |
| `--memory-reservation` | `-r` | 设置内存软限制 |
| `--memory-swap` | `-m` | 设置交换内存限制 |
| `--memory-swappiness` | `-s` | 设置交换内存调度策略 |
| `--name` | `-n` | 设置容器名称 |
| `--network` | `-n` | 设置网络 |
| `--no-healthcheck` | `-H` | 禁用健康检查 |
| `--oom-kill-disable` | `-k` | 禁用 OOM 杀手 |
| `--oom-score-adj` | `-s` | 设置 OOM 评分 |
| `--pid` | `-p` | 设置 PID 命名空间 |
| `--pids-limit` | `-l` | 设置 PIDs 限制 |
| `--privileged` | `-p` | 开启特权模式 |
| `--publish` | `-p` | 发布端口 |
| `--publish-all` | `-P` | 发布所有端口 |
| `--read-only` | `-r` | 开启只读模式 |
| `--restart` | `-r` | 设置重启策略 |
| `--rm` | `-r` | 自动删除容器 |
| `--runtime` | `-r` | 设置运行时 |
| `--security-opt` | `-S` | 设置安全选项 |
| `--shm-size` | `-s` | 设置共享内存大小 |
| `--stop-signal` | `-s` | 设置停止信号 |
| `--sysctl` | `-s` | 设置系统控制参数 |
| `--tmpfs` | `-t` | 挂载 tmpfs |
| `--tty` | `-t` | 分配伪终端 |
| `--ulimit` | `-u` | 设置 ulimit |
| `--user` | `-u` | 设置用户 |
| `--userns` | `-u` | 设置用户命名空间 |
| `--uts` | `-u` | 设置 UTS 命名空间 |
| `--volume` | `-v` | 挂载卷 |
| `--volume-driver` | `-v` | 设置卷驱动 |
| `--volumes-from` | `-v` | 挂载卷 |
| `--workdir` | `-w` | 设置工作目录 |

## 示例
```
# podman run -it --rm --name mycontainer alpine /bin/ash
```

- 运行 ubuntu 镜像，并以交互模式运行，并自动删除容器
```
# podman run -it --rm --name myubuntu ubuntu /bin/bash
```

- host 模式运行
```
podman run --rm --net=host alpine ping www.baidu.com
```


