# pdb
调试

- 进入调试
```
sudo pip install ipdb  

python -m pdb hello.py (python 内置) 

w (where): 打印当前调用堆栈

d (down): 执行跳转到当前的深一层

u (up): 执行跳转到当前的上一层

n:单步执行，next的首字母 

s:step(步进)的首字母 

r:return(返回)的首字母 

c:continue(继续)的首字母 

j num:运行到地num行，jump的首字母 

p attr:查看当前attr变量值 

l:查看运行到某处代码 

a:查看全部栈内变量 

h:帮助，help的首字母 

q:退出，quit的首字母 

b 
b  10:当前文件第10行 
b func:当前func方法 
b test/1:10:test目录1.py 文件第10行 
```

- 无法直接下端点的可以
```python
# 方到需要断点的行
import pdb
pdb.set_trace()
```
