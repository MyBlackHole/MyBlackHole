# error 
- Adding local Minio host to 'mc' configuration
```
账号长度必须大于等于5，密码长度必须大于等于8位）
```

- S3 The length of delimiter must be 1.
```shell
// Recursive: true,
```

- Unable to start the server: Insufficient permissions to use specified por
```shell
Use 'sudo setcap cap_net_bind_service=+ep /path/to/minio' to provide sufficient permissions
sudo setcap cap_net_bind_service=+ep ./minio

_MINIO_DEBUG_NO_EXIT="debug" sudo /home/black/go/bin/dlv exec ./minio -- server ./database --address :80
```
