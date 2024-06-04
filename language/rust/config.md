# config

```shell
lvim ~/.cargo/config
[source.crates-io]
registry = "https://github.com/rust-lang/crates.io-index"
 
replace-with = 'rsproxy'
# 中国科学技术大学
#replace-with = 'ustc'
 
# rsproxy
[source.rsproxy]
registry = "https://rsproxy.cn/crates.io-index"
 
[source.rsproxy-sparse]
registry = "sparse+https://rsproxy.cn/index"
 
[registries.rsproxy]
index = "https://rsproxy.cn/crates.io-index"
 
# 清华大学
[source.tuna]
registry = "https://mirrors.tuna.tsinghua.edu.cn/git/crates.io-index.git"
 
# 中国科学技术大学
[source.ustc]
registry = "https://mirrors.ustc.edu.cn/crates.io-index/"
 
# 上海交通大学
[source.sjtu]
registry = "https://mirrors.sjtug.sjtu.edu.cn/git/crates.io-index"
 
# rustcc社区
[source.rustcc]
registry = "https://code.aliyun.com/rustcc/crates.io-index.git"
 
[net]
git-fetch-with-cli=true
```
