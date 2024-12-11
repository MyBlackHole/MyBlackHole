# pprof

通过 runtime/pprof 或 net/http/pprof 启动CPU性能数据的采集。

## cpu

```go
import (
    "runtime/pprof"
    "os"
)

func main() {
    // 启动CPU性能分析，每秒采样100次
    pprof.StartCPUProfile(os.Stdout)
    defer pprof.StopCPUProfile()
    // ... 执行程序逻辑 ...
}

```


## goroutine

```go
import (
    "runtime/pprof"
)

func main() {
    // 获取当前所有Goroutine的堆栈信息
    pprof.Lookup("goroutine").WriteTo(os.Stdout, 1)
    // ... 执行程序逻辑 ...
}

```


## mem

```go

import (
    "runtime/pprof"
)

func main() {
    // 设置内存分配采样率，每分配512KB内存进行一次采样
    runtime.MemProfileRate = 512 \* 1024

    // ... 执行程序逻辑 ...
    // 停止内存分配采样
    pprof.Lookup("allocs").WriteTo(os.Stdout, 1)
}

```

## lock

```go
import (
    "runtime/pprof"
)

func main() {
    // 获取阻塞操作的采样信息
    pprof.Lookup("block").WriteTo(os.Stdout, 1)
    // ... 执行程序逻辑 ...
}

```

## 生成火焰图

```shell
paru -S graphviz
go tool pprof -pdf profile.pprof > profile.pprof.pdf
```
