# tmux

[[config]]

# 终端复用工具
 在它的帮助下，你可以在同一个控制台上建立、访问并控制多个终端
 你可以断开与一个 tmux 终端的连接，此时程序将在后台运行， 当你需要时，可以随时重新连接到这个终端

```
  tmux [command]     # 运行一条命令
                     # 如果单独使用 'tmux' 而不指定某个命令，将会建立一个新的会话
    new              # 创建一个新的会话
     -s "Session"    # 创建一个会话，并命名为“Session”
     -n "Window"     # 创建一个窗口，并命名为“Window”
     -c "/dir"       # 在指定的工作目录中启动会话
    attach           # 连接到上一次的会话（如果可用）
     -t "#"          # 连接到指定的会话
     -d              # 断开其他客户端的会话
    ls               # 列出打开的会话
     -a              # 列出所有打开的会话
    lsw              # 列出窗口
     -a              # 列出所有窗口
     -s              # 列出会话中的所有窗口
    lsp              # 列出窗格
     -a              # 列出所有窗格
     -s              # 列出会话中的所有窗格
     -t "#"          # 列出指定窗口中的所有窗格
    kill-window      # 关闭当前窗口
     -t "#"          # 关闭指定的窗口
     -a              # 关闭所有窗口
     -a -t "#"       # 关闭除指定窗口以外的所有窗口
    kill-session     # 关闭当前会话
     -t "#"          # 关闭指定的会话
     -a              # 关闭所有会话
     -a -t "#"       # 关闭除指定会话以外的所有会话
```


### 快捷键

通过“前缀”快捷键，可以控制一个已经连入的 tmux 会话。

```
----------------------------------------------------------------------
  (C-b) = Ctrl + b    # 在使用下列快捷键之前，需要按这个“前缀”快捷键
  (M-1) = Meta(super) + 1 或 Alt + 1
----------------------------------------------------------------------
  ?                  # 列出所有快捷键
  $                  # 重命名会话
  ,                  # 重命名窗口
  .                  # 移动窗口到其他会话
  :                  # 进入 tmux 的命令提示符
  r                  # 强制重绘当前客户端
  c                  # 创建一个新窗口
  !                  # 将当前窗格从窗口中移出，成为为一个新的窗口
  %                  # 将当前窗格分为左右两半
  "                  # 将当前窗格分为上下两半
  n                  # 切换到下一个窗口
  p                  # 切换到上一个窗口
  {                  # 将当前窗格与上一个窗格交换
  }                  # 将当前窗格与下一个窗格交换
  s                  # 在交互式界面中，选择并连接至另一个会话
  w                  # 在交互式界面中，选择并激活一个窗口
  0 至 9             # 选择 0 到 9 号窗口
  d                  # 断开当前客户端
  D                  # 选择并断开一个客户端
  &                  # 关闭当前窗口
  x                  # 关闭当前窗格
  Up, Down           # 将焦点移动至相邻的窗格
  Left, Right
  M-1 到 M-5         # 排列窗格：
                       # 1) 水平等分
                       # 2) 垂直等分
                       # 3) 将一个窗格作为主要窗格，其他窗格水平等分
                       # 4) 将一个窗格作为主要窗格，其他窗格垂直等分
                       # 5) 平铺
  C-Up, C-Down       # 改变当前窗格的大小，每按一次增减一个单位
  C-Left, C-Right
  M-Up, M-Down       # 改变当前窗格的大小，每按一次增减五个单位
  M-Left, M-Right
  :setw synchronise-panes on  # 开启多 pane 同步输入
```

快捷键	翻译	原文
C-b C-b	发送前缀键	Send the prefix key
C-b C-o	旋转窗格	Rotate through the panes
C-b C-z	暂停当前客户	Suspend the current client
C-b Space	选择下一个布局	Select next layout
C-b !	将窗格中断到新窗口	Break pane to a new window
C-b "	垂直分割窗口	Split window vertically
C-b #	列出所有粘贴缓冲区	List all paste buffers
C-b $	重命名当前会话	Rename current session
C-b %	水平分割视窗	Split window horizontally
C-b &	退出当前窗口	Kill current window
C-b ’	提示窗口索引选择	Prompt for window index to select
C-b (	切换到上一个客户端	Switch to previous client
C-b )	切换到下一个客户端	Switch to next client
C-b ,	重命名当前窗口	Rename current window
C-b -	删除最新的粘贴缓冲区	Delete the most recent paste buffer
C-b .	移动当前窗口	Move the current window
C-b /	描述按键绑定	Describe key binding
C-b 0	选择窗口0	Select window 0
C-b 1	选择窗口1	Select window 1
C-b 2	选择窗口2	Select window 2
C-b 3	选择窗口3	Select window 3
C-b 4	选择窗口4	Select window 4
C-b 5	选择窗口5	Select window 5
C-b 6	选择窗口6	Select window 6
C-b 7	选择窗口7	Select window 7
C-b 8	选择窗口8	Select window 8
C-b 9	选择窗口9	Select window 9
C-b :	提示输入命令	Prompt for a command
C-b ;	移至先前活动的窗格	Move to the previously active pane
C-b =	从列表中选择粘贴缓冲区	Choose a paste buffer from a list
C-b ?	列出按键绑定	List key bindings
C-b C	自定义选项	Customize options
C-b D	从列表中选择一个客户	Choose a client from a list
C-b E	均匀地展开窗格	Spread panes out evenly
C-b L	切换到最后一个客户	Switch to the last client
C-b M	清除标记的窗格	Clear the marked pane
C-b [	进入复制模式	Enter copy mode
C-b ]	粘贴最新的粘贴缓冲区	Paste the most recent paste buffer
C-b c	创建一个新窗口	Create a new window
C-b d	分离当前客户	Detach the current client
C-b f	搜索窗格	Search for a pane
C-b i	显示窗口信息	Display window information
C-b l	选择以前的当前窗口	Select the previously current window
C-b m	切换标记的窗格	Toggle the marked pane
C-b n	选择下一个窗口	Select the next window
C-b o	选择下一个窗格	Select the next pane
C-b p	选择上一个窗口	Select the previous window
C-b q	显示窗格编号	Display pane numbers
C-b r	重画当前客户端	Redraw the current client
C-b s	从列表中选择一个会话	Choose a session from a list
C-b t	显示时钟	Show a clock
C-b w	从列表中选择一个窗口	Choose a window from a list
C-b x	杀死活动窗格	Kill the active pane
C-b z	缩放活动窗格	Zoom the active pane
C-b {	将活动窗格与上方窗格交换	Swap the active pane with the pane above
C-b }	将活动窗格与下面的窗格交换	Swap the active pane with the pane below
C-b ~	显示讯息	Show messages
C-b DC	重置，以便窗口的可见部分跟随光标	Reset so the visible part of the window follows the cursor
C-b PPage	Enter copy mode and scroll up	
C-b Up	选择活动窗格上方的窗格	Select the pane above the active pane
C-b Down	选择活动窗格下面的窗格	Select the pane below the active pane
C-b Left	选择活动窗格左侧的窗格	Select the pane to the left of the active pane
C-b Right	选择活动窗格右侧的窗格	Select the pane to the right of the active pane
C-b M-1	设置水平布局	Set the even-horizontal layout
C-b M-2	设置偶数垂直布局	Set the even-vertical layout
C-b M-3	设置主水平布局	Set the main-horizontal layout
C-b M-4	设置主垂直布局	Set the main-vertical layout
C-b M-5	选择平铺的布局	Select the tiled layout
C-b M-n	选择带有警报的下一个窗口	Select the next window with an alert
C-b M-o	反向旋转窗格	Rotate through the panes in reverse
C-b M-p	选择带有警报的上一个窗口	Select the previous window with an alert
C-b M-Up	调整窗格大小为5	Resize the pane up by 5
C-b M-Down	将窗格缩小5	Resize the pane down by 5
C-b M-Left	调整左窗格大小为5	Resize the pane left by 5
C-b M-Right	将窗格右移5	Resize the pane right by 5
C-b C-Up	调整窗格大小 up	Resize the pane up
C-b C-Down	调整窗格大小 down	Resize the pane down
C-b C-Left	调整窗格大小 left	Resize the pane left
C-b C-Right	调整窗格大小right	Resize the pane right
C-b S-Up	向上移动窗口的可见部分	Move the visible part of the window up
C-b S-Down	向下移动窗口的可见部分	Move the visible part of the window down
C-b S-Left	向左移动窗口的可见部分	Move the visible part of the window left
C-b S-Right	向右移动窗口的可见部分	Move the visible part of the window right