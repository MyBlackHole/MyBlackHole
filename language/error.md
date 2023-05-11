# 

- socket 销毁后进行 读 写 关闭
```shell
# client 与 server 建立连接后, 关闭 client 的 socket, client 进行 读 写 关闭
## server
./out/obj/TCP-IP-NetworkNote_learn/ch04/echo_server 9999

## client
gdb --args ./out/obj/sys_socket_learn/shutdown_test 127.0.0.1 9999
>>> break 44
>>> r
>>> n
>>> call close(3)
>>> call read(sock, buf, 30)
$2 = -1
>>> call strerror(errno)
$3 = 0x7ffff7f585ae "Bad file descriptor"
>>> call write(sock, buf, 30)
$2 = -1
>>> call strerror(errno)
$5 = 0x7ffff7f585ae "Bad file descriptor"
>>> call close(3)
$6 = -1
>>> call strerror(errno)
$7 = 0x7ffff7f585ae "Bad file descriptor"

# client 与 server 建立连接后, 关闭 server 的 socket, client 进行 读 写 关闭
## server
gdb --args ./out/obj/TCP-IP-NetworkNote_learn/ch04/echo_server 9999
>>> break 68
Breakpoint 1 at 0x151f: file ../TCP-IP-NetworkNote_learn/ch04/echo_server.c, line 68.
>>> r


## client
gdb --args ./out/obj/sys_socket_learn/shutdown_test 127.0.0.1 9999
>>> break 44
>>> r

## server
>>> p clnt_sock
$1 = 4
>>> call close(4)
>>> p strerror(errno)
$3 = 0x7ffff7f585ae "Bad file descriptor"

## client 
会收到 SIGPIPE(网络或管道已经关闭) 信号, (根据TCP协议的规定，会收到一个RST响应，client再往这个服务器发送数据时，系统会发出一个SIGPIPE信号给进程，告诉进程这个连接已经断开了，不要再写了)
```
