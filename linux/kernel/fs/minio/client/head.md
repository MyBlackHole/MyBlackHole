# head

- 显示对象的前 n 行
```shell
mc head BH/wdg1/util_minio.py
import os
from datetime import timedelta

from minio import Minio
from minio.deleteobjects import DeleteObject
from minio.error import S3Error


class Bucket(object):
    client = None
```
