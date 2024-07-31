# stash


- stash
```shell
<!-- 备份当前的工作区的内容，从最近的一次提交中读取相关内容，让工作区保证和上次提交的内容一致。同时，将当前的工作区内容保存到Git栈中 -->
git stash

<!-- 备份工作区内容，同时添加备注信息 -->
git stash save "socket 控制需求"

<!-- 没有加 -a 这个option选项，代码开发可能是在原代码上进行修改的。而对于在项目里加入了代码新文件的开发来说，-a选项才会将新加入的代码文件同时放入暂存区 -->
git stash save -a "messeag"

<!-- 从Git栈中读取最近一次保存的内容，恢复工作区的相关内容。但是不会将该stash记录删除 -->
git stash apply

<!-- 把最近的一条stash记录删除 -->
git stash drop

<!-- 从Git栈中读取最近一次保存的内容，恢复工作区的相关内容。由于可能存在多个Stash的内容，所以用栈来管理，pop会从最近的一个stash中读取内容并恢复，同时会删除这条stash记录，相当于git stash apply和git stash drop一起执行了 -->
git stash pop

<!-- 显示Git栈内的所有备份，可以利用这个列表来决定从那个地方恢复 -->
git stash list

<!-- 清空Git栈，原来存储的所以stash的节点都消失了 -->
git stash clear
```

