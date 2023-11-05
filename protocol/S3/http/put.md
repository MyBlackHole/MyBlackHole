# put

- 桶检查
```curl
curl -k -i --raw -o 0.dat  \
    -H "Host: 192.168.100.179:9000" \
    -H "User-Agent: MinIO (linux; amd64) minio-go/v7.0.63 mc/DEVELOPMENT.2023-10-26T21-23-45Z" \
    -H "Authorization: AWS4-HMAC-SHA256 Credential=12345678/20231104/us-east-1/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=c293610b4c4245c4921730068e3f7773dcc5537031cf4216fb7428ea03bc37bf" 
    -H "X-Amz-Content-Sha256: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855" 
    -H "X-Amz-Date: 20231104T121511Z" "http://192.168.100.179:9000/wdg1/?location="

GET http://192.168.100.179:9000/wdg1/?location= HTTP/1.1
Host: 192.168.100.179:9000
User-Agent: MinIO (linux; amd64) minio-go/v7.0.63 mc/DEVELOPMENT.2023-10-26T21-23-45Z
Authorization: AWS4-HMAC-SHA256 Credential=12345678/20231104/us-east-1/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=c293610b4c4245c4921730068e3f7773dcc5537031cf4216fb7428ea03bc37bf
X-Amz-Content-Sha256: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
X-Amz-Date: 20231104T121511Z


HTTP/1.1 200 OK
Accept-Ranges: bytes
Content-Length: 128
Content-Type: application/xml
Server: MinIO
Strict-Transport-Security: max-age=31536000; includeSubDomains
Vary: Origin
Vary: Accept-Encoding
X-Amz-Id-2: dd9025bab4ad464b049177c95eb6ebf374d3b3fd1af9251148b658df7ac2e3e8
X-Amz-Request-Id: 17946A8CBFD5DFE5
X-Content-Type-Options: nosniff
X-Xss-Protection: 1; mode=block
Date: Sat, 04 Nov 2023 12:15:12 GMT

<?xml version="1.0" encoding="UTF-8"?>
<LocationConstraint xmlns="http://s3.amazonaws.com/doc/2006-03-01/"></LocationConstraint>
```

- 检查对象锁
```curl
curl -k -i --raw -o 0.dat  
    -H "Host: 192.168.100.179:9000" 
    -H "User-Agent: MinIO (linux; amd64) minio-go/v7.0.63 mc/DEVELOPMENT.2023-10-26T21-23-45Z" 
    -H "Authorization: AWS4-HMAC-SHA256 Credential=12345678/20231104/us-east-1/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=6295af312eb6e7b553d51caf7aa154e01e682f346b2df55f93c08bf31e768ab3" 
    -H "X-Amz-Content-Sha256: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855" 
    -H "X-Amz-Date: 20231104T121512Z" "http://192.168.100.179:9000/wdg1/?object-lock="

GET http://192.168.100.179:9000/wdg1/?object-lock= HTTP/1.1
Host: 192.168.100.179:9000
User-Agent: MinIO (linux; amd64) minio-go/v7.0.63 mc/DEVELOPMENT.2023-10-26T21-23-45Z
Authorization: AWS4-HMAC-SHA256 Credential=12345678/20231104/us-east-1/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=6295af312eb6e7b553d51caf7aa154e01e682f346b2df55f93c08bf31e768ab3
X-Amz-Content-Sha256: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
X-Amz-Date: 20231104T121512Z

HTTP/1.1 404 Not Found
Accept-Ranges: bytes
Content-Length: 354
Content-Type: application/xml
Server: MinIO
Strict-Transport-Security: max-age=31536000; includeSubDomains
Vary: Origin
Vary: Accept-Encoding
X-Amz-Id-2: dd9025bab4ad464b049177c95eb6ebf374d3b3fd1af9251148b658df7ac2e3e8
X-Amz-Request-Id: 17946A8CC0030508
X-Content-Type-Options: nosniff
X-Xss-Protection: 1; mode=block
Date: Sat, 04 Nov 2023 12:15:12 GMT

<?xml version="1.0" encoding="UTF-8"?>
<Error>
    <Code>ObjectLockConfigurationNotFoundError</Code>
    <Message>Object Lock configuration does not exist for this bucket</Message>
    <BucketName>wdg1</BucketName>
    <Resource>/wdg1/</Resource>
    <RequestId>17946A8CC0030508</RequestId>
    <HostId>dd9025bab4ad464b049177c95eb6ebf374d3b3fd1af9251148b658df7ac2e3e8</HostId>
</Error>
```


- 上传数据
```curl
curl -k -i --raw -o 0.dat -X PUT  \
    -H "Host: 192.168.100.179:9000" 
    -H "User-Agent: MinIO (linux; amd64) minio-go/v7.0.63 mc/DEVELOPMENT.2023-10-26T21-23-45Z" 
    -H "Authorization: AWS4-HMAC-SHA256 Credential=12345678/20231104/us-east-1/s3/aws4_request,SignedHeaders=host;x-amz-content-sha256;x-amz-date;x-amz-decoded-content-length,Signature=cbbf8408cbde4cf4e5ae433b8072116343189814359b5c23b4aea4ff26d77d6b" 
    -H "Content-Type: text/plain" -H "X-Amz-Content-Sha256: STREAMING-AWS4-HMAC-SHA256-PAYLOAD" 
    -H "X-Amz-Date: 20231104T121512Z" 
    -H "X-Amz-Decoded-Content-Length: 1024" "http://192.168.100.179:9000/wdg1/180.log"


PUT http://192.168.100.179:9000/wdg1/180.log HTTP/1.1
Host: 192.168.100.179:9000
User-Agent: MinIO (linux; amd64) minio-go/v7.0.63 mc/DEVELOPMENT.2023-10-26T21-23-45Z
Content-Length: 1198
Authorization: AWS4-HMAC-SHA256 Credential=12345678/20231104/us-east-1/s3/aws4_request,SignedHeaders=host;x-amz-content-sha256;x-amz-date;x-amz-decoded-content-length,Signature=cbbf8408cbde4cf4e5ae433b8072116343189814359b5c23b4aea4ff26d77d6b
Content-Type: text/plain
X-Amz-Content-Sha256: STREAMING-AWS4-HMAC-SHA256-PAYLOAD
X-Amz-Date: 20231104T121512Z
X-Amz-Decoded-Content-Length: 1024

400;chunk-signature=b5059ef0e93647bb036bf86728a658f12e55017da58728c4ff09717acef02c02
xxxxxxxx....
0;chunk-signature=38ffb21860be4002931402db684795fe1c7e52e03c37c0e94eec199610dd953f



HTTP/1.1 200 OK
Accept-Ranges: bytes
Content-Length: 0
ETag: "0f343b0931126a20f133d67c2b018a3b"
Server: MinIO
Strict-Transport-Security: max-age=31536000; includeSubDomains
Vary: Origin
Vary: Accept-Encoding
X-Amz-Id-2: dd9025bab4ad464b049177c95eb6ebf374d3b3fd1af9251148b658df7ac2e3e8
X-Amz-Request-Id: 17946A8CC48CC00F
X-Content-Type-Options: nosniff
X-Xss-Protection: 1; mode=block
Date: Sat, 04 Nov 2023 12:15:12 GMT
```
