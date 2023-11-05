# cp 
指的是 ossutils cp 操作

```shell

HEAD /wdg1/ossutil?objectMeta HTTP/1.1
Host: 127.0.0.1:80
User-Agent: aliyun-sdk-go/v2.2.9 (Linux/6.2.0-34-generic/x86_64;go1.20.3)/ossutil-v1.7.17
Authorization: OSS 123:mrQoYF4D3WL/iTcujG+MfM1YP3A=
Date: Sun, 05 Nov 2023 02:47:54 GMT

HTTP/1.1 400 Bad Request
Server: AliyunOSS
X-Oss-Request-Id: 5A2549E8BD1AA116C635EB42
Content-Type: application/xml
Server: WEBrick/1.8.1 (Ruby/3.1.2/2022-04-12) OpenSSL/3.0.8
Date: Sun, 05 Nov 2023 02:47:54 GMT
Content-Length: 288
Connection: Keep-Alive

PUT /wdg1/ossutil HTTP/1.1
Host: 127.0.0.1:80
User-Agent: aliyun-sdk-go/v2.2.9 (Linux/6.2.0-34-generic/x86_64;go1.20.3)/ossutil-v1.7.17
Content-Length: 11275646
Authorization: OSS 123:qKvFarvSoY2YP36gf6AySZx1WNo=
Content-Type: application/octet-stream
Date: Sun, 05 Nov 2023 02:47:54 GMT
Accept-Encoding: gzip

xxxxxxxxxxxxxxxxxxx...


HTTP/1.1 200 OK
X-Oss-Request-Id: B474E7147A35F47E9AE45D46
Server: AliyunOSS
ETag: "f5b6e11d5379c606ea28cde47aed7939"
X-Oss-Bucket-Version: 1418321259
Date: Sun, 05 Nov 2023 02:47:54 GMT
Content-Length: 0
Connection: Keep-Alive
```
