# lifecycle

## restore
- 恢复指定数据 2 天
```shell
mc ilm restore -r --days 2 LGP/back099
```

```python
[bala@f36 minio-py]$ PYTHONPATH=$PWD python
Python 3.10.5 (main, Jun  9 2022, 00:00:00) [GCC 12.1.1 20220507 (Red Hat 12.1.1-1)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> from minio import Minio
>>> import sys
>>> c = Minio("play.min.io", access_key="Q3AM3UQ867SPQQA43P2F", secret_key="zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG")
>>> c.trace_on(sys.stdout)
>>> r = c.get_bucket_lifecycle("b01")
---------START-HTTP---------
GET /b01?lifecycle= HTTP/1.1
Host: play.min.io
User-Agent: MinIO (Linux; x86_64) minio-py/7.1.10
X-Amz-Content-Sha256: UNSIGNED-PAYLOAD
X-Amz-Date: 20220627T092234Z
Authorization: AWS4-HMAC-SHA256 Credential=*REDACTED*/20220627/us-east-1/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=*REDACTED*

HTTP/1.1 200
Server: nginx/1.18.0 (Ubuntu)
Date: Mon, 27 Jun 2022 09:22:35 GMT
Content-Type: application/xml
Content-Length: 188
Connection: keep-alive
Accept-Ranges: bytes
Content-Security-Policy: block-all-mixed-content
Strict-Transport-Security: max-age=31536000; includeSubDomains
Vary: Origin
Vary: Accept-Encoding
X-Amz-Bucket-Region: us-east-1
X-Amz-Request-Id: 16FC6FDC6F2C7EEB
X-Content-Type-Options: nosniff
X-Xss-Protection: 1; mode=block

<LifecycleConfiguration><Rule><ID>casncmd2r08vkfrumc9g</ID><Status>Enabled</Status><Filter><Prefix></Prefix></Filter><Expiration><Days>1</Days></Expiration></Rule></LifecycleConfiguration>
----------END-HTTP----------
>>> from minio.commonconfig import ENABLED, Filter
>>> from minio.lifecycleconfig import Expiration, LifecycleConfig, Rule
>>> c.set_bucket_lifecycle("b01", LifecycleConfig([Rule(ENABLED, rule_filter=Filter(prefix=""), rule_id="delete_expired", expiration=Expiration(days=1))]))
---------START-HTTP---------
PUT /b01?lifecycle= HTTP/1.1
Content-Md5: XabR+/hutLCXqegQ76nIrA==
Host: play.min.io
User-Agent: MinIO (Linux; x86_64) minio-py/7.1.10
Content-Length: 223
X-Amz-Content-Sha256: UNSIGNED-PAYLOAD
X-Amz-Date: 20220627T094015Z
Authorization: AWS4-HMAC-SHA256 Credential=*REDACTED*/20220627/us-east-1/s3/aws4_request, SignedHeaders=content-length;content-md5;host;x-amz-content-sha256;x-amz-date, Signature=*REDACTED*

<LifecycleConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/"><Rule><Status>Enabled</Status><Expiration><Days>1</Days></Expiration><Filter><Prefix /></Filter><ID>delete_expired</ID></Rule></LifecycleConfiguration>

HTTP/1.1 200
Server: nginx/1.18.0 (Ubuntu)
Date: Mon, 27 Jun 2022 09:40:16 GMT
Content-Length: 0
Connection: keep-alive
Accept-Ranges: bytes
Content-Security-Policy: block-all-mixed-content
Strict-Transport-Security: max-age=31536000; includeSubDomains
Vary: Origin
Vary: Accept-Encoding
X-Amz-Bucket-Region: us-east-1
X-Amz-Request-Id: 16FC70D38BA90F94
X-Content-Type-Options: nosniff
X-Xss-Protection: 1; mode=block


----------END-HTTP----------
>>> 
```

