# build

-w 去掉调试信息
-s 去掉符号表
-X 注入变量, 编译时赋值

```shell
GOOS=linux go build -ldflags="-s -w" server.go
```
