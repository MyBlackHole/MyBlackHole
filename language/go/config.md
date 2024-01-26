# config

- GOGC
```shell
此环境变量用于设置垃圾回收的阈值, 默认值为100
表示当新分配的内存占已使用内存的百分比超过100时，触发垃圾回收
如果需要更频繁的垃圾回收，可以将其设置为较小的值，如50
```

- GODEBUG
```shell
此环境变量用于启用或禁用Go语言运行时的调试信息
其中一个选项是`gctrace=1`，用于打印垃圾回收的详细日志，包括内存分配和回收的信息
```

- 设置代理
```shell
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.io,direct
```
