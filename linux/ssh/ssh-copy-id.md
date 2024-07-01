# ssh-copy-id

免密

- 拷贝公钥到目标机器的授权文件 (authorized_keys)
```shell
ssh-copy-id root@192.168.50.189
```

# error
- No such file or directory /root/.pub
```shell
ssh-keygen -t rsa
```

- ERROR: No identities found
```shell
ssh-keygen -t rsa
```
