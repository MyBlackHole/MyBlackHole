# gem

```shell
# 添加镜像源并移除默认源 (每个用户独立的)
gem sources --add https://mirrors.tuna.tsinghua.edu.cn/rubygems/ --remove https://rubygems.org/
# 列出已有源
gem sources -l
# 应该只有镜像源一个


<!-- 或编辑 ~/.gemrc -->
https://mirrors.tuna.tsinghua.edu.cn/rubygems/
加到 sources 字段

bundle config mirror.https://rubygems.org https://mirrors.tuna.tsinghua.edu.cn/rubygems
```
