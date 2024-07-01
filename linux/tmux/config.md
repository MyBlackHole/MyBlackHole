# config

- terminal capability cm required
```shell
export TERM=screen-256color
```

## .tmux.conf

```shell
# git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm

# 启用 vi
# CTL+b+[
setw -g mode-keys vi

# List of plugins
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'
set -g @plugin 'nordtheme/tmux'
set -g @plugin 'tmux-plugins/tmux-yank'
set -g @plugin 'tmux-plugins/tmux-resurrect'
# 关闭自动恢复
# set -g @plugin 'tmux-plugins/tmux-continuum'
# set -g @continuum-restore 'on'

# Other examples:
# set -g @plugin 'github_username/plugin_name'
# set -g @plugin 'git@github.com/user/plugin'
# set -g @plugin 'git@bitbucket.com/user/plugin'
# Initialize TMUX plugin manager (keep this line at the very bottom of tmux.conf)
run -b '~/.tmux/plugins/tpm/tpm'
```


- 旧的
```
# tmux.conf 示例
# 2014.10
### 通用设置
###########################################################################
# 启用 UTF-8 编码
setw -g utf8 on
set-option -g status-utf8 on
# 命令回滚/历史数量限制
set -g history-limit 2048
# 从 1 开始编号，而不是从 0 开始
set -g base-index 1
# 启用鼠标
set-option -g mouse-select-pane on
# 重新加载配置文件
unbind r
bind r source-file ~/.tmux.conf
### 快捷键设置
###########################################################################
# 取消默认的前缀键 C-b
unbind C-b
# 设置新的前缀键 `
set-option -g prefix `
# 多次按下前缀键时，切换到上一个窗口
bind C-a last-window
bind ` last-window
# 按下F11/F12，可以选择不同的前缀键
bind F11 set-option -g prefix C-a
bind F12 set-option -g prefix `
# Vim 风格的快捷键绑定
setw -g mode-keys vi
set-option -g status-keys vi
# 使用 Vim 风格的按键在窗格间移动
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R
# 循环切换不同的窗口
bind e previous-window
bind f next-window
bind E swap-window -t -1
bind F swap-window -t +1
# 较易于使用的窗格分割快捷键
bind = split-window -h
bind - split-window -v
unbind '"'
unbind %
# 在嵌套使用 tmux 的情况下，激活最内层的会话，以便向其发送命令
bind a send-prefix
### 外观主题
###########################################################################
# 状态栏颜色
set-option -g status-justify left
set-option -g status-bg black
set-option -g status-fg white
set-option -g status-left-length 40
set-option -g status-right-length 80
# 窗格边框颜色
set-option -g pane-active-border-fg green
set-option -g pane-active-border-bg black
set-option -g pane-border-fg white
set-option -g pane-border-bg black
# 消息框颜色
set-option -g message-fg black
set-option -g message-bg green
# 窗口状态栏颜色
setw -g window-status-bg black
setw -g window-status-current-fg green
setw -g window-status-bell-attr default
setw -g window-status-bell-fg red
setw -g window-status-content-attr default
setw -g window-status-content-fg yellow
setw -g window-status-activity-attr default
setw -g window-status-activity-fg yellow
### 用户界面
###########################################################################
# 通知方式
setw -g monitor-activity on
set -g visual-activity on
set-option -g bell-action any
set-option -g visual-bell off
# 自动设置窗口标题
set-option -g set-titles on
set-option -g set-titles-string '#H:#S.#I.#P #W #T' # 窗口编号,程序名称,是否活动
# 调整状态栏
set -g status-left "#[fg=red] #H#[fg=green]:#[fg=white]#S#[fg=green] |#[default]"
# 在状态栏中显示性能计数器
# 需要用到 https://github.com/thewtex/tmux-mem-cpu-load
set -g status-interval 4
set -g status-right "#[fg=green] | #[fg=white]#(tmux-mem-cpu-load)#[fg=green] | #[fg=cyan]%H:%M #[default]"
```

