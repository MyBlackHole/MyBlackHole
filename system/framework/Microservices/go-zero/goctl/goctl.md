# goctl

## api

## docker
```shell
goctl docker
docker build -t zero-examles-bookstore-add:v0.0.1 -f Dockerfile ...
```

## rpc

- 生成一个 greet 服务
```shell
goctl rpc new greet
```

- 生成一个 proto 文件
```shell
goctl rpc -o greet.proto
```

- 根据 proto 生成 grpc 服务
```shell
goctl rpc protoc greet.proto --go_out=.  --go-grpc_out=.  --zrpc_out=.

goctl rpc protoc graceful.proto --go_out=./pb --go-grpc_out=./pb --zrpc_out=./pb
```

- 生成自动运行 dome
```shell
goctl quickstart --service-type micro
```
