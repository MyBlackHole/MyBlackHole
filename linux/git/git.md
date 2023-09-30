# git
版本管理工具
![[imgs/Pasted image 20230926124745.png]]
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

- 设置全局忽略
```shell
git config --global core.excludesfile ~/.gitignore
# 本质还是修改配置文件

.gitconfig
[core]
	excludesfile = /home/black/.gitignore
```

- 设置多个远程分支
```shell
git remote set-url my git@github.com:MyBlackHole/linux.git
```

- 取消提交
```shell
git reset --soft HEAD^ (保留更改)
git restore --staged . (取消暂存)
```


## 提交
|   |   |
|---|---|
|git commit -m [message]|提交暂存区到仓库区|
|git commit [file1] [file2] ... -m [message]|提交暂存区的指定文件到仓库区|
|git commit -a|提交工作区自上次commit之后的变化，直接到仓库区|
|git commit -v|提交时显示所有diff信息|
|git commit --amend -m [message]|如果代码没有任何新变化，则用来改写上一次commit的提交信息|
|git commit --amend [file1] [file2] ...|重做上一次commit，并包括指定文件的新变化|
|git commit -am 'xxx'|将add和commit合为一步|


## 分支

|   |   |   |
|---|---|---|
|git fetch||获取所有远程分支（不更新本地分支，另需merge）|
|git branch||列出所有本地分支|
|git branch --contains 50089||显示包含提交50089的分支|
|git branch –merged||显示所有已合并到当前分支的分支|
|git branch --no-merged||显示所有未合并到当前分支的分支|
|git branch -r||列出所有远程分支|
|git branch –a||列出所有本地分支和远程分支|
|git branch [branch-name]||新建一个分支，但依然停留在当前分支|
|git branch --track [branch] [remote-branch]||新建一个分支，与指定的远程分支建立追踪关系|
|git branch -m master master_copy||本地分支改名|
|git branch -d [branch-name]||删除分支|
|git branch -D hotfixes/BJVEP933||强制删除分支hotfixes/BJVEP933|
|git branch --set-upstream [branch] [remote-branch]||建立追踪关系，在现有分支与指定的远程分支之间|
|git push origin --delete [branch-name] git branch -dr [remote/branch]||删除远程分支|
|git checkout -b [branch] git checkout -b master master_copy||新建一个分支，并切换到该分支|
|git checkout [branch-name]||切换到指定分支，并更新工作区|
|git checkout --track hotfixes/BJVEP933||检出远程分支hotfixes/BJVEP933并创建本地跟踪分支|
|git checkout -b devel origin/develop||从远程分支develop创建新本地分支devel并检出|
|git checkout -- README||检出head版本的README文件（可用于修改错误回退）|
|git checkout -||切换到上一个分支|
|git checkout [branch] [path]|git checkout master 1.txt|拉取其他分支文件到当前分支|
|git cherry-pick [commit]||选择一个commit，合并进当前分支|
|git cherry-pick ff44785404a8e||合并提交ff44785404a8e的修改|


## 查看信息

|   |   |
|---|---|
|git status|显示有变更的文件|
|git ls-files|列出git index包含的文件|
|git ls-tree HEAD|内部命令：显示某个git对象|
|git reflog|显示所有提交，包括孤立节点|
|git grep "delete from"|文件中搜索文本“delete from”|
|git whatchange|显示提交历史对应的文件修改|
|git log git log -1  # 显示1行日志 -n为n行|显示当前分支的版本历史|
|git log --stat|显示commit历史，以及每次commit发生变更的文件|
|git log --pretty=format:'%h %s' --graph|图示提交日志|
|git log -S [keyword]|搜索提交历史，根据关键词|
|git log [tag] HEAD --pretty=format:%s|显示某个commit之后的所有变动，每个commit占据一行|
|git log [tag] HEAD --grep feature|显示某个commit之后的所有变动，其"提交说明"必须符合搜索条件|
|git log --follow [file] <br><br>git whatchanged [file]|显示某个文件的版本历史，包括文件改名|
|git log -p [file] <br><br>git log -p -m|显示指定文件相关的每一次diff|
|git log -5 --pretty --oneline|显示过去5次提交|
|git reflog|命令历史（查看未来版本）|
|git shortlog -sn|显示所有提交过的用户，按提交次数排序|
|git blame [file]|显示指定文件是什么人在什么时间修改过|
|git diff|显示暂存区和工作区的差异|
|git diff --cached [file]|显示暂存区和上一个commit的差异|
|git diff origin/master..master --stat|只显示差异的文件，不显示具体内容|
|git diff HEAD|显示工作区与当前分支最新commit之间的差异|
|git diff HEAD^|比较与上一个版本的差异|
|git diff HEAD -- ./lib|比较与HEAD版本lib目录的差异|
|git diff origin/master..master|比较远程分支master上有本地分支master上没有的|
|git diff [first-branch]...[second-branch]|显示两次提交之间的差异|
|git diff --shortstat "@{0 day ago}"|显示今天你写了多少行代码|
|git show [commit](HEAD^ : 显示HEAD的父(上一个版本)的提交日志 ^^为上两个版本 ^5为上5个版本)|显示某次提交的元数据和内容变化|
|git show --name-only [commit]|显示某次提交发生变化的文件|
|git show [commit]:[filename]|显示某次提交时，某个文件的内容|
|git show-branch|图示当前分支历史|
|git show-branch --all <br><br>git show HEAD@{5}|图示所有分支历史|
|git show master@{yesterday}|显示master分支昨天的状态|
|git reflog|显示当前分支的最近几次提交|


## 撤销

|   |   |
|---|---|
|git reset [file]|重置暂存区的指定文件，与上一次commit保持一致，但工作区不变|
|git reset --hard|重置暂存区与工作区，与上一次commit保持一致|
|git reset [commit]|重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变|
|git reset --hard [commit]|重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致|
|git reset --hard HEAD^（^:上一版本  ^^:上上版本 ~\d :回退 \d版本 ）|将当前版本重置为HEAD（通常用于merge失败回退）|
|git reset --keep [commit]|重置当前HEAD为指定commit，但保持暂存区和工作区不变|
|git revert [commit]|新建一个commit，用来撤销指定commit 后者的所有变化都将被前者抵消，并且应用到当前分支|
|git stash <br><br>git stash pop|暂时将未提交的变化移除，稍后再移入|
|git stash list|查看所有暂存|
|git stash show -p stash@{0}|参考第一次暂存|
|git stash apply stash@{0}|应用第一次暂存|


## 远程同步

|   |   |
|---|---|
|git fetch [remote]|下载远程仓库的所有变动|
|git fetch --prune|获取所有原创分支并清除服务器上已删掉的分支|
|git remote -v|显示所有远程仓库|
|git remote show [remote]|显示某个远程仓库的信息|
|git remote add [shortname] [url]|增加一个新的远程仓库，并命名|
|git pull origin master|获取远程分支master并merge到当前分支|
|git pull [remote] [branch]|取回远程仓库的变化，并与本地分支合并|
|git push --set-upstream origin 三天内数据|推送本地分支到（自动创建远程分支）远程分支|
|git push [remote] [branch]|上传本地指定分支到远程仓库|
|git push [remote] --force|强行推送当前分支到远程仓库，即使有冲突|
|git push [remote] --all|推送所有分支到远程仓库|