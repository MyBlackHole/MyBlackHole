# format-patch

- 生成 HEAD 提交的 patch 文件 (HEAD^ 之后就是 HEAD)
```shell
❯ git format-patch HEAD^
0001-f-4941710542-xxxx.patch
```

- 生成当前分支最后两个提交的 patch 文件
```shell
git format-patch HEAD^^

git format-patch -2
```
