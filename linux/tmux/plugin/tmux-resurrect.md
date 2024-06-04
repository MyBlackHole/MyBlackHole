# tmux-resurrect

tmux-resurrect 是一个 tmux 插件，
它可以帮助你在 tmux 会话中保存和恢复窗口布局、会话名称、会话选项、以及会话内的命令。

## 安装

### tpm
```shell
lvim ~/.tmux.conf
set -g @plugin 'tmux-plugins/tmux-resurrect'
```

## 操作
- 保存 tmux 会话状态
```shell
Ctrl-b Ctrl-s
```

- 恢复 tmux 会话状态
```shell
Ctrl-b Ctrl-r
```
