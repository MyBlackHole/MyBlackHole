# config

D: 显示调试信息

- 清空配置
```shell
xmake config -c -D
```

- debug 模式
```shell
xmake config -m debug
```

- 配置
```shell
xmake config --menu
```

- 构建动态库
```shell
xmake config -k shared
```

```shell
xmake config -m debug -k shared
```
