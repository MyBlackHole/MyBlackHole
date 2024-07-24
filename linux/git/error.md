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

- tag 占用, 删除分支后无法拉远程
```shell
<!-- 删除远程 tag -->
git push origin --delete tag 4.6.0.0-release

<!-- 拉取远程 分支 -->
git fetch origin 4.6.0.0-release:4.6.0.0-release
```
