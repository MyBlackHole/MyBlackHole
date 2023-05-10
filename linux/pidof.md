# pidof
用于查找指定名称进程的所有进程号
![[imgs/Pasted image 20230428015242.png]]

## 语法
```shell
pidof (选项) (参数)
```

### 选项
```shell
-s：仅返回一个进程号；
-c：仅显示具有相同“root”目录的进程；
-x：显示由脚本开启的进程；
-o：指定不显示的进程ID。
```

## 例子
- 返回ngxinPID
```shell
Pidof ngxin
```
