# uv

rust 开发的包管理器，类似 pip。

- 项目初始化
```shell
uv init exapmle

cd example

uv add ruff

uv run ruff check

uv lock

uv sync

<!--设置指定 python 版本-->
uv venv --python 3.12.0

uv run --python pypy@3.8 -- python --version
```

- pip 转换为 uv
```shell
uv init

uv add $(cat requirements.txt)

rm requirements.txt

uv synv

uv synv -U
```

- 安装多个 python 版本
```shell
uv python install 3.10 3.11
```

- 当前目录使用指定 python 版本
```shell
uv python pin 3.11
```

