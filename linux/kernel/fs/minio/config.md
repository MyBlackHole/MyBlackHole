# config

- 支持AWS4-HMAC-SHA256认证机制
```json
{
    "credential": {
        "accessKeyId": "your-access-key-id",
        "secretAccessKey": "your-secret-access-key",
        "signature": "S3v4"
    },
    "region": "us-east-1",
    "browser": "on",
    "compress": "on",
    "encrypt": "off",
    "notify": "off",
    "bucketLookup": "dns",
    "console": {
        "browser": "off",
        "banner": "off"
    }
}
```

- debug
```shell
命令选项加上
--debug

mc: <DEBUG> GET /aio-wdg/?location= HTTP/1.1
Host: ss-rgw-unicom-sxxa.superstor.cn:39443
User-Agent: MinIO (linux; amd64) minio-go/v7.0.63 mc/DEVELOPMENT.2023-10-26T21-23-45Z
Authorization: AWS4-HMAC-SHA256 Credential=**REDACTED**/20231205/us-east-1/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=**REDACTED**
X-Amz-Content-Sha256: UNSIGNED-PAYLOAD
X-Amz-Date: 20231205T035952Z
Accept-Encoding: gzip

mc: <DEBUG> HTTP/1.1 200 OK
Content-Length: 140
Connection: keep-alive
Date: Tue, 05 Dec 2023 03:59:34 GMT
Server: nginx/1.20.1
X-Amz-Request-Id: tx0000000000000002797a7-00656ea025-4409b-zone-1698204961

mc: <DEBUG> TLS Certificate found:
mc: <DEBUG>  >> Country: US
mc: <DEBUG>  >> Organization: DigiCert, Inc.
mc: <DEBUG>  >> Expires: 2024-10-20 23:59:59 +0000 UTC
mc: <DEBUG> TLS Certificate found:
mc: <DEBUG>  >> Country: US
mc: <DEBUG>  >> Organization: DigiCert Inc
mc: <DEBUG>  >> Expires: 2031-11-09 23:59:59 +0000 UTC
mc: <DEBUG> Response Time:  174.102364ms

mc: <DEBUG> HEAD /aio-wdg/ HTTP/1.1
Host: ss-rgw-unicom-sxxa.superstor.cn:39443
User-Agent: MinIO (linux; amd64) minio-go/v7.0.63 mc/DEVELOPMENT.2023-10-26T21-23-45Z
Authorization: AWS4-HMAC-SHA256 Credential=**REDACTED**/20231205/zg-1698204961/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=**REDACTED**
X-Amz-Content-Sha256: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
X-Amz-Date: 20231205T035952Z

mc: <DEBUG> HTTP/1.1 200 OK
Connection: keep-alive
Date: Tue, 05 Dec 2023 03:59:34 GMT
Server: nginx/1.20.1
X-Amz-Request-Id: tx000000000000000279799-00656ea026-436b4-zone-1698204961
X-Rgw-Bytes-Used: 1129362981
X-Rgw-Object-Count: 1734
Content-Length: 0

mc: <DEBUG> TLS Certificate found:
mc: <DEBUG>  >> Country: US
mc: <DEBUG>  >> Organization: DigiCert, Inc.
mc: <DEBUG>  >> Expires: 2024-10-20 23:59:59 +0000 UTC
mc: <DEBUG> TLS Certificate found:
mc: <DEBUG>  >> Country: US
mc: <DEBUG>  >> Organization: DigiCert Inc
mc: <DEBUG>  >> Expires: 2031-11-09 23:59:59 +0000 UTC
mc: <DEBUG> Response Time:  100.409159ms

mc: <DEBUG> GET /aio-wdg/?delimiter=%2F&encoding-type=url&fetch-owner=true&list-type=2&prefix= HTTP/1.1
Host: ss-rgw-unicom-sxxa.superstor.cn:39443
User-Agent: MinIO (linux; amd64) minio-go/v7.0.63 mc/DEVELOPMENT.2023-10-26T21-23-45Z
Authorization: AWS4-HMAC-SHA256 Credential=**REDACTED**/20231205/zg-1698204961/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=**REDACTED**
X-Amz-Content-Sha256: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
X-Amz-Date: 20231205T035952Z
Accept-Encoding: gzip

mc: <DEBUG> HTTP/1.1 200 OK
Transfer-Encoding: chunked
Connection: keep-alive
Content-Type: application/xml
Date: Tue, 05 Dec 2023 03:59:34 GMT
Server: nginx/1.20.1
X-Amz-Request-Id: tx00000000000000027977b-00656ea026-436d8-zone-1698204961

mc: <DEBUG> TLS Certificate found:
mc: <DEBUG>  >> Country: US
mc: <DEBUG>  >> Organization: DigiCert, Inc.
mc: <DEBUG>  >> Expires: 2024-10-20 23:59:59 +0000 UTC
mc: <DEBUG> TLS Certificate found:
mc: <DEBUG>  >> Country: US
mc: <DEBUG>  >> Organization: DigiCert Inc
mc: <DEBUG>  >> Expires: 2031-11-09 23:59:59 +0000 UTC
mc: <DEBUG> Response Time:  96.201508ms

[2023-12-04 18:07:49 CST]   505B STANDARD .gitignore
[2023-12-04 18:12:17 CST] 1.8MiB STANDARD openssh-9.3p1.tar.gz
[2023-12-05 11:59:52 CST]     0B bin/
[2023-12-05 11:59:52 CST]     0B include/
[2023-12-05 11:59:52 CST]     0B lib/
[2023-12-05 11:59:52 CST]     0B man/
[2023-12-05 11:59:52 CST]     0B openssh-9.3p1/
[2023-12-05 11:59:52 CST]     0B openssh-9.3p1_src/
[2023-12-05 11:59:52 CST]     0B xtrabackup-test/
```
