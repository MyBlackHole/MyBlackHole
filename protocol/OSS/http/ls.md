# ls

指的是 ossutils ls 操作

```shell
GET /wdg1/?delimiter&encoding-type=url&marker&max-keys=1000&prefix HTTP/1.1
Host: 127.0.0.1:80
User-Agent: aliyun-sdk-go/v2.2.9 (Linux/6.2.0-34-generic/x86_64;go1.20.3)/ossutil-v1.7.17
Authorization: OSS 123:uUMZ61M0tzyN22QKzE0U75RGdr0=
Date: Sun, 05 Nov 2023 02:58:00 GMT
Accept-Encoding: gzip

HTTP/1.1 200 OK
X-Oss-Request-Id: CB345A26616287CCC6403501
Server: AliyunOSS
Date: Sun, 05 Nov 2023 02:58:00 GMT
Content-Length: 611
Connection: Keep-Alive

<?xml version="1.0" encoding="UTF-8"?>
<ListBucketResult
    xmlns="http://doc.oss-cn-hangzhou.aliyuncs.com">
    <Name>wdg1</Name>
    <Prefix></Prefix>
    <Marker></Marker>
    <MaxKeys>1000</MaxKeys>
    <Delimiter></Delimiter>
    <EncodingType>url</EncodingType>
    <NextMarker>ossutil</NextMarker>
    <IsTruncated>false</IsTruncated>
    <Contents>
        <Key>ossutil</Key>
        <LastModified>2023-11-05T02:47:54.000Z</LastModified>
        <ETag>F5B6E11D5379C606EA28CDE47AED7939</ETag>
        <Type>Multipart</Type>
        <Size>11275646</Size>
        <StorageClass>Standard</StorageClass>
        <Owner>
            <ID>00220120222</ID>
            <DisplayName>1390402650033793</DisplayName>
        </Owner>
    </Contents>
</ListBucketResult>
```
