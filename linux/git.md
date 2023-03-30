# git
版本管理工具

## 配置
```shell
git config --global user.name "black"
git config --global user.email "1358244533@qq.com"
ssh-keygen -t rsa -C "1358244533@qq.com"
```

# 案例

- 终端中文乱码
```shell
git config --global core.quotepath false
```

- git 开发中查询某一行代码的提交作者
```shell
git blame <filename> - L n,m
    <filename> 为要查找的文件路径+文件名
    -L 后面的n,m代表要查找的起始行和结束行
```
