- 
SECURITY ERROR
This download does NOT match an earlier download recorded in go.sum.
The bits may have been replaced on the origin server, or an attacker may
have intercepted the download attempt.
```shell
rm go.sum
<!-- go clean -modcache -->
go mod tidy

<!-- 或 -->
<!-- 关闭 GOSUMDB -->
export GOSUMDB=off
<!-- 设置 GONOSUMDB -->
export GONOSUMDB=*.corp.example.com,rsc.io/private
```
