# error

- error: toolchain 'nightly-x86_64-unknown-linux-gnu' is not installed
```shell
rustup toolchain install nightly
```

- SSL error: received early EOF; class=Ssl (16); code=Eof (-20) 
```shell
# 在~/.cargo/config中加入 
[http] 
check-revoke = false 
```
