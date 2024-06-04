# rebase

从提交历史中删除C2 commit，而保留C3、C4的改动。
![[imgs/Pasted image 20230714205821.png]]

```shell
$ git log --pretty=format:"%h %s" HEAD~3..HEAD
a5f4a0d added cat-file
310154e updated README formatting and added blame
f7f3f6d changed my name a bit


$ git rebase -i HEAD~3
pick f7f3f6d changed my name a bit
pick 310154e updated README formatting and added blame
pick a5f4a0d added cat-file

删除commit a5f4a0d，就是把『pick a5f4a0d added cat-file』这一行删掉
保存并退出编辑器，git就把 commit a5f4a0d删掉了
```


- 合并提交
```shell
git rebase -i HEAD~3
除了第一行(必须保留第一个)，其他都改成 squash(合并到上一个提交)，然后保存退出。

git rebase --edit-todo(二次修改)
```

