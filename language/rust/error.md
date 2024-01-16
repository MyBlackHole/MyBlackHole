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

- 查询编译异常的 lib 库
```shell
cargo clean && cargo build -vv 2>/dev/null | grep 'rustc-link-lib'
```

- Test under musl failed for pthread_getname_np not found, 或者依赖 musl 编译异常 ???
```shell
RUSTFLAGS="-C target-feature=-crt-static" cargo run --target x86_64-unknown-linux-musl
```

- path matching sidecar/clash-meta-x86_64-unknown-linux-gnu not found.
```shell
???
```
