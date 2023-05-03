# lvim(neovim)
[docs](https://yianwillis.github.io/vimcdoc/doc/help.html)
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


:jumps # 跳转历史记录

# 不会修改历史记录
ctrl+i 
ctrl+o

:changes # 修改的位置
```

## 例子
- 打开远程文件
```shell
lvim sftp://root@192.168.78.202//home/goldendb/db1/bin/restore.py
```
