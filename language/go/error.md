# error

- linkmode requires external (cgo) linking, but cgo is not enabled
```shell
源码拉取直接编译正常
奇怪,算了不重要
```

- 
SECURITY ERROR
This download does NOT match an earlier download recorded in go.sum.
The bits may have been replaced on the origin server, or an attacker may
have intercepted the download attempt.
```shell
rm go.sum
<!-- go clean -modcache -->
go mod tidy

<!-- 或 -->
<!-- 关闭 GOSUMDB -->
export GOSUMDB=off
<!-- 设置 GONOSUMDB -->
export GONOSUMDB=*.corp.example.com,rsc.io/private
```

- no such file or directory
```shell
alpine 对 golang 编译结果支持有问题
alpine 不支持动态连接, 加上 CGO_ENABLED=0
CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" server.go
```
