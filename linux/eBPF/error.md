# error
- Unable to find clang libraries
```shell
<!-- clang 与 libclang 版本不一致 -->
sudo apt install libclang-14-dev
```

- *** 没有规则可制作目标“/usr/lib/llvm-14/lib/libPolly.a”，由“src/cc/libbcc.so.0.29.1” 需求。 停止。
```shell
sudo apt install libpolly-14-dev
```
