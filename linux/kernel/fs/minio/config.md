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
