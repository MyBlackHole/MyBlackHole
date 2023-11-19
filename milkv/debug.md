# debug
[kgdb]

```shell
sudo nvim buildroot-2021.05/board/milkv/duo/overlay/mnt/system/ko/loadsystemko.sh

<!-- 开启 KGDB 会堵塞 -->
<!-- 注释 -->
# insmod /mnt/system/ko/cv180x_jpeg.ko
# insmod /mnt/system/ko/cv180x_clock_cooling.ko
```

- kgdb serial
```shell
CONFIG_DEBUG_INFO=y
CONFIG_DEBUG_KERNEL=y
CONFIG_FRAME_POINTER=y
CONFIG_MAGIC_SYSRQ=y
CONFIG_MAGIC_SYSRQ_SERIAL=y
CONFIG_KGDB_SERIAL_CONSOLE=y
CONFIG_KGDB_KDB=y
CONFIG_KGDB=y

<!-- 会堵塞,原因未定位 -->
```
