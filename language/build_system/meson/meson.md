# meson
构建系统

## install
```shell
sudo apt-get install meson

pip3 install meson ninja
```

## 命令

### configure
查看支持哪些编译选项
```shell
meson configure
```


## dome
```shell
# 推荐参数设置方式
CFLAGS="-g -O0 -lcrypto" meson build

# 有问题, TODO 有可能不生效,没空查
meson build -Dc_args="-g -O0"

# 或
meson setup build


cd build
ninja
```

- 增量配置构建
```shell
CFLAGS="-g -O0 -lcrypto" meson -Dexamples=all --reconfigure build
```

- 编译
```shell
meson compile -C build
```
