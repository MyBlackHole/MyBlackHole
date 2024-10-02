# commit

 提交

|   |   |
|---|---|
|git commit -m [message]|提交暂存区到仓库区|
|git commit [file1] [file2] ... -m [message]|提交暂存区的指定文件到仓库区|
|git commit -a|提交工作区自上次commit之后的变化，直接到仓库区|
|git commit -v|提交时显示所有diff信息|
|git commit --amend -m [message]|如果代码没有任何新变化，则用来改写上一次commit的提交信息|
|git commit --amend [file1] [file2] ...|重做上一次commit，并包括指定文件的新变化|
|git commit -am 'xxx'|将add和commit合为一步|



- 配置git的commit模板
```
git config --global commit.template ~/.gitcommit.template

git config --global core.editor nvim

# 弹出编辑器编辑模板
git commit

cat ~/.gitcommit.template
【f-4941710542】[rpc] [fix] fix rpc client bug

# type 字段包含:
# feat：新功能（feature）
# fix：修补bug
# docs：文档（documentation）
# style： 格式（不影响代码运行的变动）
# refactor：重构（即不是新增功能，也不是修改bug的代码变动）
# test：增加测试
# chore：构建过程或辅助工具的变动
# scope：用于说明 commit 影响的范围，比如数据层、控制层、视图层等等。
# subject：是 commit 目的的简短描述，不超过50个字符
# Body：部分是对本次 commit 的详细描述，可以分成多行
# Footer：用来关闭 Issue或以BREAKING CHANGE开头，后面是对变动的描述、以及变动理由和迁移方法
```
