# error
异常处理

- Bad owner or permissions on /home/black/.ssh/config
```shell
chmod 644 ~/.ssh/config
```

- fatal: detected dubious ownership in repository at '/home/black/Documents/aio-cdm'
To add an exception for this directory, call:
```shell
git config --global --add safe.directory /home/black/Documents/aio-cdm
```

- SSL_read: SSL_ERROR_SYSCALL
```shell
git config --global http.sslVerify "false"
```
