# xmake

```shell
$(arch) 架构/系统
$(os)	获取当前编译平台的操作系统	>= 2.0.1
$(host)	获取本机操作系统	>= 2.0.1
$(tmpdir)	获取临时目录	>= 2.0.1
$(curdir)	获取当前目录	>= 2.0.1
$(buildir)	获取构建输出目录	>= 2.0.1
$(scriptdir)	获取工程描述脚本目录	>= 2.1.1
$(globaldir)	获取全局配置目录	>= 2.0.1
$(configdir)	获取本地工程配置目录	>= 2.0.1
$(programdir)	xmake安装脚本目录	>= 2.1.5
$(projectdir)	获取工程根目录	>= 2.0.1
$(shell)	执行外部shell命令	>= 2.0.1
$(env)	获取外部环境变量	>= 2.1.5
$(reg)	获取windows注册表配置项的值	>= 2.1.5
```

- 生成 c 工程 console 类型
```shell
xmake create -l c -t console test

xmake create -l c -t tbox.static test
```

- 查找系统包(mysqlclient)信息
```shell
xmake l find_package mysqlclient
```

- 编译 (-D 显示详情信息)
```shell
xmake -D
```

- 静态链接
```shell
add_ldflags("-static")
```

- 强制编译
```shell
xmake -P .
```

- 配置交叉编译
```shell
xmake config -m debug --cross=aarch64-linux-gnu- -c
# -a aarch64 指定输出架构目录名
xmake config -m debug --cross=aarch64-linux-gnu- -a aarch64 -c
```

