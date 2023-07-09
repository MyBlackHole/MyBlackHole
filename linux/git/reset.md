# reset
- mixed 
意思是：不删除工作空间改动代码，撤销commit，并且撤销git add . 操作
这个为默认参数,git reset --mixed HEAD^ 和 git reset HEAD^ 效果是一样的。

- soft
不删除工作空间改动代码，撤销commit，不撤销git add . 

- hard
删除工作空间改动代码，撤销commit，撤销git add . 
