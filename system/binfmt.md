# binfmt

支持二进制格式转换的系统调用。

```shell
paru -S qemu-user-static-binfmt

❯ cat /proc/sys/fs/binfmt_misc/qemu-aarch64
enabled
interpreter /usr/bin/qemu-aarch64-static
flags: PF
offset 0
magic 7f454c460201010000000000000000000200b700
mask ffffffffffffff00fffffffffffffffffeffffff

paru -S qemu-user-binfmt
❯ cat /proc/sys/fs/binfmt_misc/qemu-aarch64
enabled
interpreter /usr/bin/qemu-aarch64
flags: PF
offset 0
magic 7f454c460201010000000000000000000200b700
mask ffffffffffffff00fffffffffffffffffeffffff


❯ qemu-aarch64-static -L /usr/aarch64-linux-gnu/ ./s3fs --help
./s3fs: error while loading shared libraries: libfuse.so.2: cannot open shared object file: No such file or directory
```
