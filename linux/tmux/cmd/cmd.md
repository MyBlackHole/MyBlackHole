# cmd

```shell
attach-session       attach                   -- 附加或切换到会话
bind-key             bind                     -- 将键绑定到命令
break-pane           breakp                   -- 将现有窗格打破到新窗口中
capture-pane         capturep                 -- 将窗格的内容捕获到缓冲区
choose-buffer                                 -- 将窗格置于缓冲区选择模式
choose-client                                 -- 将窗口置于客户端选择模式
choose-tree                                   -- 将窗口置于树选择模式
clear-history        clearhist                -- 删除并清除窗格的历史记录
clock-mode                                    -- 进入时钟模式
command-prompt                                -- 在客户端中打开 tmux 命令提示符
confirm-before       confirm                  -- 运行命令但之前要求确认
copy-mode                                     -- 进入复制模式
customize-mode                                -- 定制模式--进入定制模式
delete-buffer        deleteb                  -- 删除粘贴缓冲区
detach-client        detach                   -- 将客户端与服务器分离
display-menu         menu                     -- 显示菜单
display-message      display                  -- 在状态行中显示一条消息
display-panes        displayp                 -- 显示每个可见窗格的指示器
display-popup        popup                    -- 在窗格上显示弹出框
find-window          findw                    -- 在窗口中搜索模式
has-session          has                      -- 检查并报告服务器上是否存在会话
if-shell             if                       -- 如果 shell 命令成功则执行 tmux 命令
join-pane            move-pane  joinp  movep  -- 拆分窗格并将现有窗格移至新空间
kill-pane            killp                    -- 销毁给定的窗格
kill-server                                   -- 杀死客户端、会话和服务器
kill-session                                  -- 销毁给定的会话
kill-window          killw                    -- 销毁给定窗口
last-pane            lastp                    -- 选择先前选择的窗格
last-window          last                     -- 选择之前选择的窗口
link-window          linkw                    -- 将一个窗口链接到另一个窗口
list-buffers         lsb                      -- 列出会话的粘贴缓冲区
list-clients         lsc                      -- 列出连接到服务器的客户端
list-commands        lscm                     -- 列出支持的子命令
list-keys            lsk                      -- 列出所有键绑定
list-panes           lsp                      -- 列出窗口的窗格
list-sessions        ls                       -- 列出服务器管理的会话
list-windows         lsw                      -- 列出会话的窗口
load-buffer          loadb                    -- 将文件加载到粘贴缓冲区中
lock-client          lockc                    -- 锁定客户端
lock-server          lock                     -- 锁定连接到服务器的所有客户端
lock-session         locks                    -- 锁定附加到会话的所有客户端
move-window          movew                    -- 将一个窗口移动到另一个窗口
new-session          new                      -- 创建一个新会话
new-window           neww                     -- 创建一个新窗口
next-layout          nextl                    -- 将窗口移动到下一个布局
next-window          next                     -- 移动到会话中的下一个窗口
paste-buffer         pasteb                   -- 将粘贴缓冲区插入到窗口中
pipe-pane            pipep                    -- 将窗格中的输出通过管道传输到 shell 命令
previous-layout      prevl                    -- 将窗口移动到上一个布局
previous-window      prev                     -- 移至会话中的上一个窗口
refresh-client       refresh                  -- 刷新客户端
rename-session       rename                   -- 重命名会话
rename-window        renamew                  -- 重命名窗口
resize-pane          resizep                  -- 调整窗格大小
resize-window        resizew                  -- 调整窗口大小
respawn-pane         respawnp                 -- 重用命令已退出的窗格
respawn-window       respawnw                 -- 重用命令已退出的窗口
rotate-window        rotatew                  -- 旋转窗口中窗格的位置
run-shell            run                      -- 执行命令而不创建新窗口
save-buffer          saveb                    -- 将粘贴缓冲区保存到文件
select-layout        selectl                  -- 选择窗格的布局
select-pane          selectp                  -- 使窗格成为活动窗格
select-window        selectw                  -- 选择一个窗口
send-keys            send                     -- 将密钥发送到窗口
send-prefix                                   -- 将前缀键发送到窗口
server-info          info                     -- 显示服务器信息
set-buffer           setb                     -- 设置粘贴缓冲区的内容
set-environment      setenv                   -- （取消）设置环境变量
set-hook                                      -- 设置命令的钩子
set-option           set                      -- 设置会话选项
set-window-option    setw                     -- 设置窗口选项
show-buffer          showb                    -- 显示粘贴缓冲区的内容
show-environment     showenv                  -- 显示环境
show-hooks                                    -- 显示全局钩子列表
show-messages        showmsgs                 -- 显示客户端的消息日志
show-options         show                     -- 显示会话选项
show-window-options  showw                    -- 显示窗口选项
source-file          source                   -- 从文件执行 tmux 命令
split-window         splitw                   -- 将一个窗格分成两个
start-server         start                    -- 启动 tmux 服务器
suspend-client       suspendc                 -- 挂起客户端
swap-pane            swapp                    -- 交换两个窗格
swap-window          swapw                    -- 交换两个窗口
switch-client        switchc                  -- 将客户端切换到另一个会话
unbind-key           unbind                   -- 取消绑定键
unlink-window        unlinkw                  -- 取消链接窗口
wait-for             wait                     -- 等待事件或触发它
```
