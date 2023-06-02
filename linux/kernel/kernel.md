# kernel

## source 安装
```shell
sudo apt-get install kernel-source-xxxx
```

## 生成 compile_commands.json
```shell
scripts/clang-tools/gen_compile_commands.py
```

- 清理全部 make 的产物
```shell
make mrproper
```
