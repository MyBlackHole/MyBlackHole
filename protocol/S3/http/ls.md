# ls

- get 桶是否存在
```curl
<!-- request -->
curl -k -i --raw -o 0.dat  \
    -H "Host: 192.168.100.179:9000" \
    -H "User-Agent: MinIO (linux; amd64) minio-go/v7.0.63 mc/DEVELOPMENT.2023-10-26T21-23-45Z" 
    -H "Authorization: AWS4-HMAC-SHA256 Credential=12345678/20231104/us-east-1/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=d874aa6b783e93fdc4aefb9e30a1f1c870a68664f765ec71a9a4eedd4645efc1" \
    -H "X-Amz-Content-Sha256: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855" 
    -H "X-Amz-Date: 20231104T115224Z" "http://192.168.100.179:9000/wdg1/?location="

GET http://192.168.100.179:9000/wdg1/?location= HTTP/1.1
Host: 192.168.100.179:9000
User-Agent: MinIO (linux; amd64) minio-go/v7.0.63 mc/DEVELOPMENT.2023-10-26T21-23-45Z
Authorization: AWS4-HMAC-SHA256 Credential=12345678/20231104/us-east-1/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=d874aa6b783e93fdc4aefb9e30a1f1c870a68664f765ec71a9a4eedd4645efc1
X-Amz-Content-Sha256: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
X-Amz-Date: 20231104T115224Z


<!-- response -->
HTTP/1.1 200 OK
Accept-Ranges: bytes
Content-Length: 128
Content-Type: application/xml
Server: MinIO
Strict-Transport-Security: max-age=31536000; includeSubDomains
Vary: Origin
Vary: Accept-Encoding
X-Amz-Id-2: dd9025bab4ad464b049177c95eb6ebf374d3b3fd1af9251148b658df7ac2e3e8
X-Amz-Request-Id: 1794694E4DE2C513
X-Content-Type-Options: nosniff
X-Xss-Protection: 1; mode=block
Date: Sat, 04 Nov 2023 11:52:24 GMT

<?xml version="1.0" encoding="UTF-8"?>
<LocationConstraint
    xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
</LocationConstraint>
```


- head
```curl
<!-- request -->
curl -k -i --raw -o 0.dat -I  \
    -H "Host: 192.168.100.179:9000" 
    -H "User-Agent: MinIO (linux; amd64) minio-go/v7.0.63 mc/DEVELOPMENT.2023-10-26T21-23-45Z" 
    -H "Authorization: AWS4-HMAC-SHA256 Credential=12345678/20231104/us-east-1/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=7ca04405899c9c051f8a43af4df47518c416cef40c5eb13c07d5326aa88ea376" 
    -H "X-Amz-Content-Sha256: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855" 
    -H "X-Amz-Date: 20231104T115224Z" "http://192.168.100.179:9000/wdg1/util_minio.py"

HEAD http://192.168.100.179:9000/wdg1/util_minio.py HTTP/1.1
Host: 192.168.100.179:9000
User-Agent: MinIO (linux; amd64) minio-go/v7.0.63 mc/DEVELOPMENT.2023-10-26T21-23-45Z
Authorization: AWS4-HMAC-SHA256 Credential=12345678/20231104/us-east-1/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=7ca04405899c9c051f8a43af4df47518c416cef40c5eb13c07d5326aa88ea376
X-Amz-Content-Sha256: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
X-Amz-Date: 20231104T115224Z


<!-- response -->
HTTP/1.1 200 OK
Accept-Ranges: bytes
Content-Length: 6714
Content-Type: application/zip
ETag: "69ef9e68c54122cc12a59f7569b388bf"
Last-Modified: Thu, 02 Nov 2023 14:57:11 GMT
Server: MinIO
Strict-Transport-Security: max-age=31536000; includeSubDomains
Vary: Origin
Vary: Accept-Encoding
X-Amz-Id-2: dd9025bab4ad464b049177c95eb6ebf374d3b3fd1af9251148b658df7ac2e3e8
X-Amz-Request-Id: 1794694E4E01494A
X-Content-Type-Options: nosniff
X-Xss-Protection: 1; mode=block
Date: Sat, 04 Nov 2023 11:52:40 GMT

```

- get 对象
```curl
<!-- request -->
<!-- list-type=2 v2 版本 -->
curl -k -i --raw -o 0.dat  \
    -H "Host: 192.168.100.179:9000" 
    -H "User-Agent: MinIO (linux; amd64) minio-go/v7.0.63 mc/DEVELOPMENT.2023-10-26T21-23-45Z" \
    -H "Authorization: AWS4-HMAC-SHA256 Credential=12345678/20231104/us-east-1/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=1bc843bc93175e385a091c0ab6bd43d0bb30c6bca19d44b5903f02912d1e44ad" 
    -H "X-Amz-Content-Sha256: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855" \
    -H "X-Amz-Date: 20231104T115240Z" "http://192.168.100.179:9000/wdg1/?delimiter=%2F&encoding-type=url&fetch-owner=true&list-type=2&prefix=util_minio.py"

GET http://192.168.100.179:9000/wdg1/?delimiter=%2F&encoding-type=url&fetch-owner=true&list-type=2&prefix=util_minio.py HTTP/1.1
Host: 192.168.100.179:9000
User-Agent: MinIO (linux; amd64) minio-go/v7.0.63 mc/DEVELOPMENT.2023-10-26T21-23-45Z
Authorization: AWS4-HMAC-SHA256 Credential=12345678/20231104/us-east-1/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=1bc843bc93175e385a091c0ab6bd43d0bb30c6bca19d44b5903f02912d1e44ad
X-Amz-Content-Sha256: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
X-Amz-Date: 20231104T115240Z

<!-- response -->
HTTP/1.1 200 OK
Accept-Ranges: bytes
Content-Length: 631
Content-Type: application/xml
Server: MinIO
Strict-Transport-Security: max-age=31536000; includeSubDomains
Vary: Origin
Vary: Accept-Encoding
X-Amz-Id-2: dd9025bab4ad464b049177c95eb6ebf374d3b3fd1af9251148b658df7ac2e3e8
X-Amz-Request-Id: 179469520D5B7F96
X-Content-Type-Options: nosniff
X-Xss-Protection: 1; mode=block
Date: Sat, 04 Nov 2023 11:52:41 GMT

<?xml version="1.0" encoding="UTF-8"?>
<ListBucketResult
    xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
    <Name>wdg1</Name>
    <Prefix>util_minio.py</Prefix>
    <KeyCount>1</KeyCount>
    <MaxKeys>1000</MaxKeys>
    <Delimiter>/</Delimiter>
    <IsTruncated>false</IsTruncated>
    <Contents>
        <Key>util_minio.py</Key>
        <LastModified>2023-11-02T14:57:11.187Z</LastModified>
        <ETag>&#34;69ef9e68c54122cc12a59f7569b388bf&#34;</ETag>
        <Size>6714</Size>
        <Owner>
            <ID>02d6176db174dc93cb1b899f7c6078f08654445fe8cf1b6ce98d8855f66bdbf4</ID>
            <DisplayName>minio</DisplayName>
        </Owner>
        <StorageClass>STANDARD</StorageClass>
    </Contents>
    <EncodingType>url</EncodingType>
</ListBucketResult>
```
