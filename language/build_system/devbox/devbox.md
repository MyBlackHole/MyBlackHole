# devbox

是一个可以让你轻松地创建隔离环境的 shell 与 container 的命令行工具。
首先定义你开发环境中所需的软件包列表，随后 Devbox 使用该定义来为你的应用程序创建一个隔离的环境。

使用 Devbox，你可以从 Nix 软件包注册表中安装超过 400,000 个软件包版本。
Devbox最初由 Jetify 进行开发，其内部由 nix 驱动。

## 安装
```shell
# 依赖 nix
paru -S devbox nix
systemctl start nix-daemon
sudo chmod ugo=rwx /nix/var/nix/daemon-socket
```
