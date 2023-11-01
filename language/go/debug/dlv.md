# dlv
- install
```shell
go install github.com/go-delve/delve/cmd/dlv@latest
```

## exec
```shell
dlv exec ./ossutil -- -e 127.0.0.1 -i UYHTraPHdzTnsmyWWTkR -k Tzm9nDpkofV8PU0auy64LE4hH4q6bxXi9bTrlbdK ls oss://test1
```

- 在 nacos init 下断点
```shell
break github.com/zeromicro/zero-contrib/zrpc/registry/nacos.init.0
```

- 在 nacos builder 结构的 Build 方法下断点
```shell
break github.com/zeromicro/zero-contrib/zrpc/registry/nacos.(*builder).Build
```

- 针对行下断点
```shell
break pread.go:138
```

## test

- 调试单元测试
```shell
dlv test . -- -test.v -test.run TestFxSplit
```
