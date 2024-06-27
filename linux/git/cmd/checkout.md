# checkout


- 冲突处理，保留自己的
```shell
git checkout --ours ./dir
```

- 冲突处理，保留别人的
```shell
git checkout --theirs ./dir
```

- 冲突处理，保留远程的
```shell
git checkout origin/master -- ./dir
```
