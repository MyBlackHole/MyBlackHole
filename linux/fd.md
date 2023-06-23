# fd

```
Find 替代品

USAGE:

    fd [FLAGS/OPTIONS] [<pattern>] [<path>...]

FLAGS:

    -H, --hidden            搜索隐藏的文件和目录

    -I, --no-ignore         不要忽略 .(git | fd)ignore 文件匹配

        --no-ignore-vcs     不要忽略.gitignore文件的匹配

    -s, --case-sensitive    区分大小写的搜索（默认值：智能案例）

    -i, --ignore-case       不区分大小写的搜索（默认值：智能案例）

    -F, --fixed-strings     将模式视为文字字符串

    -a, --absolute-path     显示绝对路径而不是相对路径

    -L, --follow            遵循符号链接

    -p, --full-path         搜索完整路径（默认值：仅限 file-/dirname）

    -0, --print0            用null字符分隔结果

    -h, --help              打印帮助信息

    -V, --version           打印版本信息

OPTIONS:

    -d, --max-depth <depth>        设置最大搜索深度（默认值：无）

    -t, --type <filetype>...       按类型过滤：文件（f），目录（d），符号链接（l），

                                   可执行（x），空（e）

    -e, --extension <ext>...       按文件扩展名过滤

    -x, --exec <cmd>               为每个搜索结果执行命令

    -E, --exclude <pattern>...     排除与给定glob模式匹配的条目

        --ignore-file <path>...    以.gitignore格式添加自定义忽略文件

    -c, --color <when>             何时使用颜色：never，*auto*, always

    -j, --threads <num>            设置用于搜索和执行的线程数

    -S, --size <size>...           根据文件大小限制结果。

ARGS:

    <pattern>    the search pattern, a regular expression (optional)

    <path>...    the root directory for the filesystem search (optional)
```

## 例子
- 跳过 git 忽略文件搜索
```shell
fd --no-ignore musl
```
