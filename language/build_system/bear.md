# bear

- compile_commands.json 生成
```shell
bear -- make
```

- 内核 modules compile_commands.json 生成
```shell
bear -- make -j`nproc` -C /lib/modules/${KERNEL_RELEASE}/build M=$(pwd) modules
```
