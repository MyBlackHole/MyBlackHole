# chezmoi

dotfiles 配置管理

[细节](https://www.chezmoi.io/reference/commands/init/)

## config

```shell
<!-- 自动提交 -->
# ~/.config/chezmoi/chezmoi.toml

[sourceVCS]
    autoCommit = true
    autoPush = false
```

## init
```shell
<!-- 会在~/.local/share/chezmoi创建一个git仓库 -->
chezmoi init

<!-- 拉取已有仓库与指定分支 -->
chezmoi init git@github.com:MyBlackHole/Mackup.git --branch ubuntu
```

## apply
应用 git 仓库配置到本系统
```shell
chezmoi apply
```

## update
从 git 恢复到本地系统
```shell
chezmoi update
```

## add
添加文件或目录
```shell
chezmoi add .nanorc # 添加文件
chezmoi add -x .config/fish/functions/ # 添加文件夹
chezmoi add -xa .config/fish/functions # 递归添加文件夹和子目录下的全部内容
chezmoi add -T .xprofile # 添加临时内容
chezmoi add .tmux.conf --follow # 添加软链接对应的原始内容，而不是软链接符号

```

## managed
chezmoi managed # 列出所管理的内容路径

## edit
编辑
chezmoi edit ~/.bashrc --apply

## source
同步|提交

```shell
chezmoi source pull -- --rebase && chezmoi diff
chezmoi source add .nanorc
chezmoi source commit -- -m "Initial commit"
```

## apply
合并
chezmoi apply --verbose

## archive
打包
chezmoi archive --output=dotfiles.tar
