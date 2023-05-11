### rust
```shell
# 安装
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh 
```


- 完全静态二进制
```shell
# 安装 musl 支持
rustup target add x86_64-unknown-linux-musl

# 安装工具链
rustup target add x86_64-unknown-linux-musl --toolchain=nightly

cargo build --target x86_64-unknown-linux-musl
```
