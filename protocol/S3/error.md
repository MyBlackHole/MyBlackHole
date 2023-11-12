# error

- The provided &#39;x-amz-content-sha256&#39; header does not match what was computed.
```shell
minio/api.py:
     3 ▏⋅⋅⋅▏⋅⋅⋅if⋅creds:↴
     2 ▏⋅⋅⋅▏⋅⋅⋅▏⋅⋅⋅if⋅self._base_url.is_https:↴
     1 ▏⋅⋅⋅▏⋅⋅⋅▏⋅⋅⋅▏⋅⋅⋅sha256⋅=⋅"UNSIGNED-PAYLOAD"↴
  195  ▏⋅⋅⋅▏⋅⋅⋅▏⋅⋅⋅▏⋅⋅⋅sha256⋅=⋅sha256_hash(body)↴ (就是他了)
     1 ▏⋅⋅⋅▏⋅⋅⋅▏⋅⋅⋅▏⋅⋅⋅md5sum⋅=⋅None⋅if⋅md5sum_added⋅else⋅md5sum_hash(body)↴
     2 ▏⋅⋅⋅▏⋅⋅⋅▏⋅⋅⋅else:↴
     3 ▏⋅⋅⋅▏⋅⋅⋅▏⋅⋅⋅▏⋅⋅⋅sha256⋅=⋅sha256_hash(body)↴
     4 ▏⋅⋅⋅▏⋅⋅⋅else:↴
```
