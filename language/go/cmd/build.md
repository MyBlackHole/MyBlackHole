# build

-w 去掉调试信息
-s 去掉符号表
-X 注入变量, 编译时赋值

```shell
GOOS=linux go build -ldflags="-s -w" server.go
```

- 静态编译
```shell
CGO_ENABLED=0 GOOS=linux go build
```

- 提供更多调试信息
```shell
go build -gcflags="all=-N -l"
```
