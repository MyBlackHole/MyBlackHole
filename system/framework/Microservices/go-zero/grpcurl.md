# grpcurl


- 查看服务列表
```shell
grpcurl -plaintext 127.0.0.1:8080 list
graceful.GraceService
grpc.health.v1.Health
grpc.reflection.v1.ServerReflection
grpc.reflection.v1alpha.ServerReflection
```

- 查看某个服务的方法列表
```shell
grpcurl -plaintext 127.0.0.1:8080 list graceful.GraceService
graceful.GraceService.grace
```

- 查看方法定义
```shell
grpcurl -plaintext 127.0.0.1:8080 describe graceful.GraceService
graceful.GraceService is a service:
service GraceService {
  rpc grace ( .graceful.Request ) returns ( .graceful.Response );
}
```

- 查看请求参数定义
```shell
grpcurl -plaintext 127.0.0.1:8080 describe graceful.Request
graceful.Request is a message:
message Request {
  string from = 1;
}
```


- 请求服务
```shell
grpcurl -d '{"from": "BlackHole"}' -plaintext 127.0.0.1:8080 graceful.GraceService.grace
{
  "host": "My BlackHole"
}

<!-- @ 表示从标准输入读取 json 参数 -->
grpcurl -d @ -plaintext 127.0.0.1:8080 graceful.GraceService.grace
```

