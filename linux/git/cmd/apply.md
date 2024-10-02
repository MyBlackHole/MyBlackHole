# apply

- 显示 patch 的文件改动状态：
```shell
git apply --stat 0001-f-4941710542-fs-cli.patch
```

- 检查 patch 是否正常执行，大概相当于 dry-run。该命令能够检查出 patch 合并时一些常见的问题，比如 patch 中对文件进行了修改但文件不存在，patch 中创建文件但项目中已存在同名的文件。
```shell
git apply --check 0001-f-4941710542-fs-cli.patch
```

- 合并一个已经被应用的补丁。
```shell
git am 0001-f-4941710542-fs-cli.patch
```
