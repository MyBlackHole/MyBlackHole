# reset
- mixed 
意思是：不删除工作空间改动代码，撤销commit，并且撤销git add . 操作
这个为默认参数,git reset --mixed HEAD^ 和 git reset HEAD^ 效果是一样的。

- soft
不删除工作空间改动代码，撤销commit，不撤销git add . 

- hard
删除工作空间改动代码，撤销commit，撤销git add . 


- 远程重置本地当前
```shell
git fetch --all (同步远程所有到本地 origin)
git reset --hard origin/master
git pull

或

git reset --hard HEAD^
git pull
```
