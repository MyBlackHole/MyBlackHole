# mb

指的是 ossutils mb 操作

- 先检查
```shell
GET /wdg1/?delimiter&encoding-type=url&marker&max-keys=1000&prefix HTTP/1.1
Host: 127.0.0.1:80
User-Agent: aliyun-sdk-go/v2.2.9 (Linux/6.2.0-34-generic/x86_64;go1.20.3)/ossutil-v1.7.17
Authorization: OSS 123:eiBrToZYsGs3p/PxIkX0FsKQkK8=
Date: Sun, 05 Nov 2023 02:25:18 GMT
Accept-Encoding: gzip

HTTP/1.1 404 Not Found
Server: AliyunOSS
X-Oss-Request-Id: 6DAB2FD08C733F773DE922DD
Content-Type: application/xml
Server: WEBrick/1.8.1 (Ruby/3.1.2/2022-04-12) OpenSSL/3.0.8
Date: Sun, 05 Nov 2023 02:25:18 GMT
Content-Length: 313
Connection: Keep-Alive

<?xml version="1.0" encoding="UTF-8"?>
<Error>
  <Code>NoSuchBucket</Code>
  <Message>The bucket does not exist.</Message>
  <RequestId>6DAB2FD08C733F773DE922DD</RequestId>
  <HostId>oss.aliyun.com</HostId>
  <BucketName></BucketName>
</Error>
```

- 创建
```shell
PUT /wdg1/ HTTP/1.1
Host: 127.0.0.1:80
User-Agent: aliyun-sdk-go/v2.2.9 (Linux/6.2.0-34-generic/x86_64;go1.20.3)/ossutil-v1.7.17
Content-Length: 92
Authorization: OSS 123:ciYY4++dtqYxvjJ2dLvOgvfhR2o=
Content-Type: text/plain; charset=utf-8
Date: Sun, 05 Nov 2023 02:25:39 GMT
Accept-Encoding: gzip

<CreateBucketConfiguration><StorageClass>Standard</StorageClass></CreateBucketConfiguration>


HTTP/1.1 200 OK
X-Oss-Request-Id: 321E45E95C9582DBD7CF7205
Server: AliyunOSS
Location: http://127.0.0.1/wdg1/oss-example
Access-Control-Allow-Origin: *
Date: Sun, 05 Nov 2023 02:25:39 GMT
Content-Length: 0
Connection: Keep-Alive

```
