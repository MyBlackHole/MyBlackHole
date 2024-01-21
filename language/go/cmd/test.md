# test

go test

## testing.T
单元测试

- 测试当前目录指定函数
```shell
go test -v -run 测试函数名字
```

- 测试本地 mod 里所有测试案例
```shell
go test ./mod
```

- 指定函数测试
```shell
go test . -v -test.run TestFxSplit
```

- 指定 mod 与函数
```shell
go test ./cmd -v -test.run TestAllBitrotAlgorithms
```

## testing.B
压力测试

- bench
```shell
go test -bench BenchmarkFx

go test -v -bench=. benchmark_test.go

go test -v -bench=. -benchtime=5s benchmark_test.go
```
