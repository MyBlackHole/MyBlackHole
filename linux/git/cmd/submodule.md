# submodule
子模块

```shell
git submodule update --init --recursive
```

```shell
git submodule add <url> <path>
```

```shell
git submodule add -b <branch> <url> <path>
```

- add 指定分支
```shell
git submodule add -b cnn_tf_v1.10_compatible https://github.com/tensorflow/benchmarks.git 3rdparty/benchmarks
```

- 合并版本
```shell
git submodule init
git submodule update
```

- 同步 url 配置
```shell
git submodule sync
```
