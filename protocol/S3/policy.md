# policy

[细节](https://docs.amazonaws.cn/AmazonS3/latest/userguide/access-policy-language-overview.html)

```json
{
"Version":"s3.v1", 
"Id": "samplepolicy",
"Statement" : [
    {
        "Sid":"AddPerm",  
        "Effect":"Allow", 
        "Principal" : {  
            "AWS":["111122223333","444455556666","iam::111122223333:3984935484"]
         },
        "Action":["s3:*"],  
        "Resource":"arn:aws:s3:::bucket/*",    
        "Condition": {   
           "IpAddress": {"aws:SourceIp": ["54.240.143.0/24","2001:DB8:1234:5678::/64","1.1.1.1"]},
           "NotIpAddress": {"aws:SourceIp": "54.240.143.188/32"},
           "StringLike": {
				     "aws:Referer": ["*.uuci.net",""],
				     "aws:Host": ["fly.uuci.net",""]			        
           }
    },
   {},...
 ]
}
```

* 字段说明
![[imgs/policy.png]]

|Action|API|Level|
|---|---|---|
|s3:DeleteBucket|DELETE Bucket|Bucket|
|s3:ListBucket|GET Bucket(List Objects)，HEAD Bucket|Bucket|
|s3:GetBucketLocation|Get bucket location|Bucket|
|s3:ListBucketMultipartUploads|List Multipart Uploads|Bucket|
|s3:DeleteObject|DELETE Object, DELETE Multiple Object|Object|
|s3:GetObject|GET Object, HEAD Object|Object|
|s3:PutObject|PUT Object, POST Object, Initiate Multipart Upload, Upload Part, Complete Multipart Upload, PUT Object-Copy|Object|
|s3:AbortMultipartUpload|Abort Multipart Upload|Object|
|s3:ListMultipartUploadParts|List Parts|Object|
|s3:*|All API above|Bucket、Object|


- test 所有API的权限
```json
{
    "Version": "",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:ListAllMyBuckets",
                "s3:ListBucket",
                "s3:PutObject",
                "s3:DeleteObject",
                "s3:GetBucketLocation"
            ],
            "Resource": [
                "arn:aws:s3:::test/*"
            ]
        }
    ]
}
```
