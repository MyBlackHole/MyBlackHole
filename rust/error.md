### 异常案例
- SSL error: received early EOF; class=Ssl (16); code=Eof (-20) 
```shell
# 在~/.cargo/config中加入 
[http] 
check-revoke = false 
```
