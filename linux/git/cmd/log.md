# log

1. git log filename
可以看到该文件相关的commit记录

2. git log -p filename
可以显示该文件每次提交的diff

3. git show comit_id filename
可以查看某次提交中的某个文件变化

4. git show commit_id
查看某次提交

5. gitk --follow filename
以图形化界面的方式显示修改列表

6. git log -S rpc_download_file
搜索提交中包含指定字符串的提交记录

- 生成 changlog 文件
```
git log --pretty=format:"%h - %an, %ar : %s" > CHANGELOG.md

# 生成指定版本到当前版本的 changlog 文件
git log 4.3.0.2-release HEAD --pretty=format:"%h - %an, %ar : %s" > CHANGELOG.md
```

