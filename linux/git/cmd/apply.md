# apply

- 显示 patch 的文件改动状态：
```shell
git apply --stat 0001-f-4941710542-fs-cli.patch
```

- 检查 patch 是否正常执行，大概相当于 dry-run。该命令能够检查出 patch 合并时一些常见的问题，比如 patch 中对文件进行了修改但文件不存在，patch 中创建文件但项目中已存在同名的文件。
```shell
git apply --check 0001-f-4941710542-fs-cli.patch
```

- 应用 patch。
```shell
git apply 0001-f-4941710542-fs-cli.patch
```

- 应用 patch 并生成一个新的 commit。
```shell
git apply --commit 0001-f-4941710542-fs-cli.patch
```

- 应用 patch 但拒绝那些冲突的文件。
```shell
git apply --reject ../0001-f-5045245647.patch

git status
On branch 4.10.0.0-release
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   kernel/sys/sys_hook.c
        modified:   rpc/logger.cpp

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        fs-backup/fsclient/main.cpp.rej
        rpc/main.cpp.rej

no changes added to commit (use "git add" and/or "git commit -a")

```

- 合并一个已经被应用的补丁。
```shell
git am 0001-f-4941710542-fs-cli.patch
```
