# rpcinfo

主要用途是利用RPC调用，访问RPC服务器，显示其响应信息，从而查询已注册的RPC服务

## 语法
rpcinfo [-m | -s] [host]
rpcinfo -p [host]
rpcinfo -T transport host prognum [versnum]
rpcinfo -l [-T transport] host prognum [versnum]
rpcinfo [-n portnum] -u host prognum [versnum]
rpcinfo [-n portnum] [-t] host prognum [versnum]
rpcinfo -a serv_address -T transport prognum [versnum]
rpcinfo -b [-T transport] prognum versnum
rpcinfo -d [-T transport] prognum versnum

## 例子

