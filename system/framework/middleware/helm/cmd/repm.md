# repm


添加、列出、删除、更新和索引chart仓库

## add

- 添加 harbor 源
```shell
helm3 repo add harbor https://helm.goharbor.io
```

## list

- 源列表
```shell
helm3 repo list
NAME    URL
harbor  https://helm.goharbor.io
```

## update
从chart仓库中更新本地可用chart的信息

```shell
helm repo update [REPO1 [REPO2 ...]] [flags]
```
