# bread

- 给某个包的函数下断点
```shell
break github.com/minio/mux.(*routeRegexp).Match
break github.com/minio/mux.(*Router).Match

dlv break [filename]:[line number] if [condition]
```
