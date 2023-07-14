- ERROR [root] Error: Can't locate revision identified by 'efaaf700ef7b'
```shell
# 处理 efaaf700ef7b 丢失
flask db revision --rev-id efaaf700ef7b
flask db migrate 
flask db upgrade 
```
