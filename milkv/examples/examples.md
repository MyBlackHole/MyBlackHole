# examples

- 官方案例
```shell
git clone git@github.com:milkv-duo/duo-examples.git
```

- 构建
```shell
cd duo-examples
❯ ./envsetup.sh
script_dir: /run/media/black/Data/Documents/github/demo/Milkv/duo-examples
Select Product:
1. Duo (CV1800B)
2. Duo256M (SG2002) or DuoS (SG2000)
Which would you like: 1
CHIP: CV180X
ARCH: riscv64
......


cd hello-world
make

scp helloworld root@192.168.42.1:/root/

[root@milkv]~# ./helloworld
Hello, World!
```
