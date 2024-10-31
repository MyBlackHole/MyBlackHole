# Merge

- 合并模板
```
git config --global commit.template ~/.gitmerge.template

git config --global core.editor nvim
```

- 合并子工程
```shell
// 所有子工程切换到 master 分支
git submodule foreach git checkout master

// 所有子工程更新代码
git submodule foreach git pull

git add 所有子工程目录

//这里的提交应该是更新commit id
git commit -m "update submodule"

//使其保持最新，与master相同
```
