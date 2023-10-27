# object

```shell
kubectl -n rook-ceph get secret rook-ceph-object-user-my-store-cosi -o jsonpath='{.data.AccessKey}' | base64 --decode
ATMK63GDP9EMOGNQVI24
kubectl -n rook-ceph get secret rook-ceph-object-user-my-store-cosi -o jsonpath='{.data.SecretKey}' | base64 --decode
afZDtXp83xSZ6anYyNe66siYfht7mIRCJE7SPhPM
```
