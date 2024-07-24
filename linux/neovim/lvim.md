# lvim(neovim)

[docs](https://yianwillis.github.io/vimcdoc/doc/help.html)

![[imgs/lvim.png]]


## install
```shell
LV_BRANCH='release-1.4/neovim-0.9' bash <(curl -s https://raw.githubusercontent.com/LunarVim/LunarVim/release-1.4/neovim-0.9/utils/installer/install.sh)
```

```vim
:r! {command} # 将命令{command}的标准输出插入到光标下
```

- 跳转
```
gg  # 到文件头
G   # 到文件尾


Ngg # 到N行
NG  #
:N  #

gi  # 返回上一次插入文本的地方。

gd       # 跳转到局部定义
gf       # 跳转到文件

ctrl+]    # 找到光标所在位置的标签定义的地方
ctrl+t    # 回到跳转之前的标签处

<c-w> + ] # 分屏跳转到定义

<c-w> + } # 预览窗口跳转到定义
<c-w> + z # 关闭预览窗口

ctrl + ↑ ↓ ← →  修改窗口大小 

:jumps # 跳转历史记录

# 不会修改历史记录
ctrl+i 
ctrl+o

ctrl + w + ] # 分割窗口并跳转到光标下的标签
ctrl + w + L # 移动窗口到右边

:changes # 修改的位置
```

## 例子
- 打开远程文件
```shell
lvim sftp://root@192.168.78.202//home/goldendb/db1/bin/restore.py
```

- 修改字符集
```shell
set fileencodings=utf-8,ucs-bom,gb18030,gbk,gb2312,cp936
set termencoding=utf-8
set encoding=utf-8

# encoding—-该选项使用于缓冲的文本(你正在编辑的文件)，寄存器，Vim 脚本文件等等。你可以把 'encoding' 选项当作是对 Vim 内部运行机制的设定。
# fileencoding—-该选项是vim写入文件时采用的编码类型。
# termencoding—-该选项代表输出到客户终端（Term）采用的编码类型。
```

- 启动耗时检查
```shell
lvim --startuptime <file>
```

- 跳转到文件指定字节位置
```shell
光标到文件 1222 字节位置
:1222go
```
