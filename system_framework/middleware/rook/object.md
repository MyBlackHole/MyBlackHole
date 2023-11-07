# object

```shell
kubectl apply -f object.yaml
kubectl apply -f rgw-external.yaml

kubectl -n rook-ceph get cephobjectstore external-store

<!-- kubectl -n rook-ceph edit services rook-ceph-rgw-my-store -->
kubectl -n rook-ceph edit services rook-ceph-rgw-my-store-external
NodePort

kubectl -n rook-ceph get secret rook-ceph-object-user-my-store-cosi -o jsonpath='{.data.AccessKey}' | base64 --decode
T2DXDE18CHC9KJZ7P5TR
kubectl -n rook-ceph get secret rook-ceph-object-user-my-store-cosi -o jsonpath='{.data.SecretKey}' | base64 --decode
zxCAjtT7zN7axr6rXGzajJTGNsr3yeSAQsW0SAOc
```


## test
```shell
<!-- <!-- 块存储（RBD）测试 --> -->
<!-- 创建 StorageClass -->
cd rook/deploy/examples
# 创建一个名为replicapool的rbd pool
kubectl apply -f csi/rbd/storageclass.yaml

<!-- 部署 WordPress -->
kubectl apply -f mysql.yaml
kubectl apply -f wordpress.yaml

<!-- <!-- 文件系统 （CephFS） 测试 --> -->
<!-- 创建 StorageClass -->
kubectl apply -f csi/cephfs/storageclass.yaml

<!-- <!-- 对象存储 （RGW） 测试 --> -->
<!-- 创建对象存储 -->
kubectl create -f object.yaml

# 验证rgw pod正常运行
kubectl -n rook-ceph get pod -l app=rook-ceph-rgw

<!-- 创建对象存储 user -->
kubectl create -f object-user.yaml

<!-- 获取 accesskey secretkey -->
# 获取AccessKey
kubectl -n rook-ceph get secret rook-ceph-object-user-my-store-my-user -o yaml | grep AccessKey | awk '{print $2}' | base64 --decode

# 获取 SecretKey
kubectl -n rook-ceph get secret rook-ceph-object-user-my-store-my-user -o yaml | grep SecretKey | awk '{print $2}' | base64 --decode

<!-- 部署 rgw nodeport -->
kubectl apply -f rgw-external.yaml

kubectl -n rook-ceph get service rook-ceph-rgw-my-store rook-ceph-rgw-my-store-external

<!-- 通过 api 接口使用 Ceph 对象存储 -->
#首先，我们需要安装 python-boto 包，用于测试连接 S3。：
yum install python-boto -y

# 然后，编写 python 测试脚本。
# cat s3.py
#!/usr/bin/python

import boto
import boto.s3.connection
access_key = 'C7492VVSL8O11NZBK3GT'
secret_key = 'lo8IIwMfmow4fjkSOMbjebmgjzTRBQSO7w83SvBd'
conn = boto.connect_s3(
    aws_access_key_id = access_key,
    aws_secret_access_key = secret_key,
    host = '192.168.182.110', port=30369,
    is_secure=False,
    calling_format = boto.s3.connection.OrdinaryCallingFormat(),
)
bucket = conn.create_bucket('my-first-s3-bucket')
for bucket in conn.get_all_buckets():
        print "{name}\t{created}".format(
                name = bucket.name,
                created = bucket.creation_date,
)
```
