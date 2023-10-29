# hey

压测工具
[案例](https://blog.csdn.net/lsc_2019/article/details/118550151)

```shell
用法：hey [选项…] <url>

选项：
-n 请求次数。默认为 200。
-c 并发工作线程数。请确保总请求数不小于并发数量。默认为 50。
-q 每个工作线程的查询每秒限制（QPS）。默认不设限制。
-z 应用发送请求的持续时间。达到指定时间后，应用将停止并退出。如果指定了该选项，则忽略-n选项。示例：-z 10s -z 3m。
-o 输出类型。如果未提供，则打印摘要。"csv" 是唯一支持的替代选项。将响应指标以逗号分隔值格式转储。

-m HTTP 方法，GET、POST、PUT、DELETE、HEAD、OPTIONS 之一。
-H 自定义 HTTP 标头。您可以通过重复该标志来指定多个标头。例如，-H "Accept：text/html" -H "Content-Type：application/xml"。
-t 每个请求的超时时间（秒）。默认为 20，使用 0 表示无限期。
-A HTTP Accept 标头。
-d HTTP 请求体。
-D 从文件中读取 HTTP 请求体。例如，/home/user/file.txt 或 ./file.txt。
-T 内容类型，默认为 "text/html"。
-a 基本身份验证，格式为：用户名:密码。
-x HTTP 代理地址，格式为 host:port。
-h2 启用 HTTP/2。

-host HTTP Host 标头。

-disable-compression 禁用压缩。
-disable-keepalive 禁用 keep-alive，防止在不同的 HTTP 请求之间重用 TCP 连接。
-disable-redirects 禁用 HTTP 重定向跟随。
-cpus 使用的 CPU 核心数。
（当前设备的默认值为 8 个核心
```


## 例子
```shell
hey -z 60s http://localhost:8080/good
```
