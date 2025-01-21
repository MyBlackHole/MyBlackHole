# qemu

## install
```shell
paru -S qemu-full
paru -S bc qemu-full ncurses-devel
```


## 命令
- 退出
```shell
ctrl+a x # exit qemu
```

- kdump
```shell
ctrl+a c
dump-guest-memory -z xxx-vmcore
```
