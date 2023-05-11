# git
版本管理工具

## 配置
```shell
git config --global user.name "black"
git config --global user.email "1358244533@qq.com"
ssh-keygen -t rsa -C "1358244533@qq.com"
```

# 案例

- 终端中文乱码
```shell
git config --global core.quotepath false
```

- git 开发中查询某一行代码的提交作者
```shell
git blame <filename> - L n,m
    <filename> 为要查找的文件路径+文件名
    -L 后面的n,m代表要查找的起始行和结束行
```

- 清理未跟踪文件
```shell
git clean -df
```

- git 设置 ssh 代理
```shell
Host *.savannah.gnu.org *.github.com github.com
  ProxyCommand=nc -X 5 -x localhost:1080 %h %p
```

- stash
```shell
git stash: 备份当前的工作区的内容，从最近的一次提交中读取相关内容，让工作区保证和上次提交的内容一致。同时，将当前的工作区内容保存到Git栈中。
git stash save ‘message’：备份工作区内容，同时添加备注信息。
git stash save -a “messeag” ：没有加 -a 这个option选项，代码开发可能是在原代码上进行修改的。而对于在项目里加入了代码新文件的开发来说，-a选项才会将新加入的代码文件同时放入暂存区。
git stash apply: 从Git栈中读取最近一次保存的内容，恢复工作区的相关内容。但是不会将该stash记录删除
git stash drop: 把最近的一条stash记录删除。
git stash pop: 从Git栈中读取最近一次保存的内容，恢复工作区的相关内容。由于可能存在多个Stash的内容，所以用栈来管理，pop会从最近的一个stash中读取内容并恢复，同时会删除这条stash记录，相当于git stash apply和git stash drop一起执行了。
git stash list: 显示Git栈内的所有备份，可以利用这个列表来决定从那个地方恢复。
git stash clear: 清空Git栈，原来存储的所以stash的节点都消失了。
```
