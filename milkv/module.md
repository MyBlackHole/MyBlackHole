# module

```shell
cd build
source mikvsetup.sh
defconfig cv1800b_milkv_duo_sd

build_all

<!-- 打包 img -->
pack_sd_image

ssh root@192.168.42.1

<!-- 注意 app libc 库是否存在 -->
```
