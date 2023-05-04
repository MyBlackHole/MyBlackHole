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

r (return): 执行当前运行函数到结束

j num:运行到地num行，jump的首字母 

unt (until): 执行到下一行(跳出循环),或者当前堆栈结束

p attr:查看当前attr变量值 

pp attr:格式化 查看当前attr变量值 

l:查看运行到某处代码 

a:查看全部栈内变量, 列出当前执行函数的函数

h:帮助，help的首字母 

q:退出，quit的首字母 

b 
    b  10:当前文件第10行 
    b func:当前func方法 
    b test/1:10:test目录1.py 文件第10行 

threak (temporary break): 临时断点,一次性

condition bpnumber conditon，给断点设置条件，当参数condition返回True的时候bpnumber断点有效，否则bpnumber断点无效

disable：停用断点，参数为bpnumber，和cl的区别是，断点依然存在，只是不启用

enable：激活断点，参数为bpnumber

cl (clear): 
    cl:清除所有断点 
    cl bpnumber1 ...: 清除断点号
    cl line_no: 清除当前脚本 lineno 行的断点
    cl filename:line_no :清除 filename 脚本 lineno 行的断点
```

- 无法直接下端点的可以
```python
# 方到需要断点的行
import pdb
pdb.set_trace()
```

- 下断点
```shell
break /home/black/.local/lib/python3.10/site-packages/kombu/connection.py:191
```
