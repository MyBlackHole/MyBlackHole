# restore test

```shell
mc ilm restore --debug -r --days 2 LGP/back099/HAS_VastbaseG100_x86_64_V1.4_2022072614.zip
mc: <DEBUG> GET /back099/?location= HTTP/1.1
Host: ss-rgw-unicom-sxxa.superstor.cn:39443
User-Agent: MinIO (linux; amd64) minio-go/v7.0.63 mc/DEVELOPMENT.2023-10-26T21-23-45Z
Authorization: AWS4-HMAC-SHA256 Credential=**REDACTED**/20231205/us-east-1/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=**REDACTED**
X-Amz-Content-Sha256: UNSIGNED-PAYLOAD
X-Amz-Date: 20231205T040653Z
Accept-Encoding: gzip

mc: <DEBUG> HTTP/1.1 200 OK
Content-Length: 140
Connection: keep-alive
Date: Tue, 05 Dec 2023 04:06:35 GMT
Server: nginx/1.20.1
X-Amz-Request-Id: tx0000000000000002799be-00656ea1cb-4409b-zone-1698204961

mc: <DEBUG> TLS Certificate found:
mc: <DEBUG>  >> Country: US
mc: <DEBUG>  >> Organization: DigiCert, Inc.
mc: <DEBUG>  >> Expires: 2024-10-20 23:59:59 +0000 UTC
mc: <DEBUG> TLS Certificate found:
mc: <DEBUG>  >> Country: US
mc: <DEBUG>  >> Organization: DigiCert Inc
mc: <DEBUG>  >> Expires: 2031-11-09 23:59:59 +0000 UTC
mc: <DEBUG> Response Time:  157.22901ms

mc: <DEBUG> GET /back099/?delimiter=&encoding-type=url&fetch-owner=true&list-type=2&prefix=HAS_VastbaseG100_x86_64_V1.4_2022072614.zip HTTP/1.1
Host: ss-rgw-unicom-sxxa.superstor.cn:39443
User-Agent: MinIO (linux; amd64) minio-go/v7.0.63 mc/DEVELOPMENT.2023-10-26T21-23-45Z
Authorization: AWS4-HMAC-SHA256 Credential=**REDACTED**/20231205/zg-1698204961/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=**REDACTED**
X-Amz-Content-Sha256: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
X-Amz-Date: 20231205T040653Z
Accept-Encoding: gzip

mc: <DEBUG> HTTP/1.1 200 OK
Transfer-Encoding: chunked
Connection: keep-alive
Content-Type: application/xml
Date: Tue, 05 Dec 2023 04:06:35 GMT
Server: nginx/1.20.1
X-Amz-Request-Id: tx0000000000000002799ad-00656ea1cb-436b4-zone-1698204961

mc: <DEBUG> TLS Certificate found:
mc: <DEBUG>  >> Country: US
mc: <DEBUG>  >> Organization: DigiCert, Inc.
mc: <DEBUG>  >> Expires: 2024-10-20 23:59:59 +0000 UTC
mc: <DEBUG> TLS Certificate found:
mc: <DEBUG>  >> Country: US
mc: <DEBUG>  >> Organization: DigiCert Inc
mc: <DEBUG>  >> Expires: 2031-11-09 23:59:59 +0000 UTC
mc: <DEBUG> Response Time:  90.063444ms

mc: <DEBUG> POST /back099/HAS_VastbaseG100_x86_64_V1.4_2022072614.zip?restore= HTTP/1.1
Host: ss-rgw-unicom-sxxa.superstor.cn:39443
User-Agent: MinIO (linux; amd64) minio-go/v7.0.63 mc/DEVELOPMENT.2023-10-26T21-23-45Z
Content-Length: 162
Authorization: AWS4-HMAC-SHA256 Credential=**REDACTED**/20231205/zg-1698204961/s3/aws4_request, SignedHeaders=content-md5;host;x-amz-content-sha256;x-amz-date, Signature=**REDACTED**
Content-Md5: evTWSndc1YPdr7twRk0vbw==
X-Amz-Content-Sha256: 575ef5a9404ed7c4ff83669545ec216b2deca8870d4f7c592a2dd9594129faf8
X-Amz-Date: 20231205T040653Z
Accept-Encoding: gzip

mc: <DEBUG> HTTP/1.1 200 OK
Content-Length: 0
Connection: keep-alive
Date: Tue, 05 Dec 2023 04:06:35 GMT
Server: nginx/1.20.1
X-Amz-Request-Id: tx000000000000000279994-00656ea1cb-436d8-zone-1698204961

mc: <DEBUG> TLS Certificate found:
mc: <DEBUG>  >> Country: US
mc: <DEBUG>  >> Organization: DigiCert, Inc.
mc: <DEBUG>  >> Expires: 2024-10-20 23:59:59 +0000 UTC
mc: <DEBUG> TLS Certificate found:
mc: <DEBUG>  >> Country: US
mc: <DEBUG>  >> Organization: DigiCert Inc
mc: <DEBUG>  >> Expires: 2031-11-09 23:59:59 +0000 UTC
mc: <DEBUG> Response Time:  50.076696ms

Sent restore requests to 1 object(s)...
mc: <DEBUG> GET /back099/?delimiter=&encoding-type=url&fetch-owner=true&list-type=2&prefix=HAS_VastbaseG100_x86_64_V1.4_2022072614.zip HTTP/1.1
Host: ss-rgw-unicom-sxxa.superstor.cn:39443
User-Agent: MinIO (linux; amd64) minio-go/v7.0.63 mc/DEVELOPMENT.2023-10-26T21-23-45Z
Authorization: AWS4-HMAC-SHA256 Credential=**REDACTED**/20231205/zg-1698204961/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=**REDACTED**
X-Amz-Content-Sha256: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
X-Amz-Date: 20231205T040653Z
Accept-Encoding: gzip

mc: <DEBUG> HTTP/1.1 200 OK
Transfer-Encoding: chunked
Connection: keep-alive
Content-Type: application/xml
Date: Tue, 05 Dec 2023 04:06:35 GMT
Server: nginx/1.20.1
X-Amz-Request-Id: tx0000000000000002799bf-00656ea1cb-4409b-zone-1698204961

mc: <DEBUG> TLS Certificate found:
mc: <DEBUG>  >> Country: US
mc: <DEBUG>  >> Organization: DigiCert, Inc.
mc: <DEBUG>  >> Expires: 2024-10-20 23:59:59 +0000 UTC
mc: <DEBUG> TLS Certificate found:
mc: <DEBUG>  >> Country: US
mc: <DEBUG>  >> Organization: DigiCert Inc
mc: <DEBUG>  >> Expires: 2031-11-09 23:59:59 +0000 UTC
mc: <DEBUG> Response Time:  89.766882ms

mc: <DEBUG> HEAD /back099/HAS_VastbaseG100_x86_64_V1.4_2022072614.zip HTTP/1.1
Host: ss-rgw-unicom-sxxa.superstor.cn:39443
User-Agent: MinIO (linux; amd64) minio-go/v7.0.63 mc/DEVELOPMENT.2023-10-26T21-23-45Z
Authorization: AWS4-HMAC-SHA256 Credential=**REDACTED**/20231205/zg-1698204961/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=**REDACTED**
X-Amz-Content-Sha256: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
X-Amz-Date: 20231205T040653Z

mc: <DEBUG> HTTP/1.1 200 OK
Content-Length: 89433564
Accept-Ranges: bytes
Connection: keep-alive
Content-Type: application/zip
Date: Tue, 05 Dec 2023 04:06:35 GMT
Etag: "99839e33021d8964b392a13d91775462-6"
Last-Modified: Tue, 31 Oct 2023 01:33:30 GMT
Server: nginx/1.20.1
X-Amz-Request-Id: tx0000000000000002799ae-00656ea1cb-436b4-zone-1698204961
X-Amz-Restore: ongoing-request="false", expiry-date="Thu, 07 Dec 2023 04:06:35 GMT"
X-Amz-Storage-Class: GLACIER
X-Rgw-Object-Type: Normal
X_amz_archive_flags: 19
X_amz_archive_location: [{"library":"xianyuntest0fz2ghno0","serial_no":"Mcc00396","is_primary":true,"devices":[{"name":"0489F14A3A4A80_0","barcode":"","status":1,"available":1}]}]

mc: <DEBUG> TLS Certificate found:
mc: <DEBUG>  >> Country: US
mc: <DEBUG>  >> Organization: DigiCert, Inc.
mc: <DEBUG>  >> Expires: 2024-10-20 23:59:59 +0000 UTC
mc: <DEBUG> TLS Certificate found:
mc: <DEBUG>  >> Country: US
mc: <DEBUG>  >> Organization: DigiCert Inc
mc: <DEBUG>  >> Expires: 2031-11-09 23:59:59 +0000 UTC
mc: <DEBUG> Response Time:  38.716911ms

1/1 object(s) successfully restored..
```
