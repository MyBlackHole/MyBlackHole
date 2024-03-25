# run

d: 开始 gdb

- 运行项目
```shell
xmake run -d 
```

- 执行指定目标
```shell
<!-- 执行 netdb_learn -->
xmake run netdb_learn getaddrinfo
```

- gdb debug 指定目标
```shell
<!-- 执行 netdb_learn -->
xmake run -d netdb_learn getaddrinfo
```

