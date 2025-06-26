# config


- 配置镜像源
```shell
sudo nvim /etc/containers/registries.conf
unqualified-search-registries = ["docker.io"]
```

- 配置运行其他架构的镜像
```shell
paru -S qemu-user-static qemu-user-static-binfmt
<!-- paru -S qemu-user qemu-user-binfmt qemu-user-static qemu-user-static-binfmt -->

systemctl retstart systemd-binfmt

❯ ls -alh /proc/sys/fs/binfmt_misc/
Permissions Size User Date Modified Name
.rw-r--r--     0 root 18 6月  13:40  qemu-aarch64
.rw-r--r--     0 root 18 6月  13:40  qemu-aarch64_be
.rw-r--r--     0 root 18 6月  13:40  qemu-alpha
.rw-r--r--     0 root 18 6月  13:40  qemu-arm
.rw-r--r--     0 root 18 6月  13:40  qemu-armeb
.rw-r--r--     0 root 18 6月  13:40  qemu-hexagon
.rw-r--r--     0 root 18 6月  13:40  qemu-hppa
.rw-r--r--     0 root 18 6月  13:40  qemu-loongarch64
.rw-r--r--     0 root 18 6月  13:40  qemu-m68k
.rw-r--r--     0 root 18 6月  13:40  qemu-microblaze
.rw-r--r--     0 root 18 6月  13:40  qemu-microblazeel
.rw-r--r--     0 root 18 6月  13:40  qemu-mips
.rw-r--r--     0 root 18 6月  13:40  qemu-mips64
.rw-r--r--     0 root 18 6月  13:40  qemu-mips64el
.rw-r--r--     0 root 18 6月  13:40  qemu-mipsel
.rw-r--r--     0 root 18 6月  13:40  qemu-mipsn32
.rw-r--r--     0 root 18 6月  13:40  qemu-mipsn32el
.rw-r--r--     0 root 18 6月  13:40  qemu-or1k
.rw-r--r--     0 root 18 6月  13:40  qemu-ppc
.rw-r--r--     0 root 18 6月  13:40  qemu-ppc64
.rw-r--r--     0 root 18 6月  13:40  qemu-ppc64le
.rw-r--r--     0 root 18 6月  13:40  qemu-riscv32
.rw-r--r--     0 root 18 6月  13:40  qemu-riscv64
.rw-r--r--     0 root 18 6月  13:40  qemu-s390x
.rw-r--r--     0 root 18 6月  13:40  qemu-sh4
.rw-r--r--     0 root 18 6月  13:40  qemu-sh4eb
.rw-r--r--     0 root 18 6月  13:40  qemu-sparc
.rw-r--r--     0 root 18 6月  13:40  qemu-sparc32plus
.rw-r--r--     0 root 18 6月  13:40  qemu-sparc64
.rw-r--r--     0 root 18 6月  13:40  qemu-xtensa
.rw-r--r--     0 root 18 6月  13:40  qemu-xtensaeb
.-w-------     0 root 18 6月  13:38  register
.rw-r--r--     0 root 18 6月  13:38  status

❯ podman pull --platform linux/arm64 ubuntu

❯ podman run -it --rm --platform linux/arm64 ubuntu uname -m
aarch64
```
