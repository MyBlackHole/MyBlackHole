# kdb

## 配置
```shell
CONFIG_DEBUG_INFO=y
CONFIG_FRAME_POINTER=y
CONFIG_MAGIC_SYSRQ=y
CONFIG_MAGIC_SYSRQ_SERIAL=y
CONFIG_KGDB_SERIAL_CONSOLE=y
CONFIG_KGDB_KDB=y
CONFIG_KGDB=y


kgdboc= ttyAMA0,115200
<!-- 或系统启动后 -->
echo "ttyAMA0,115200" > /sys/module/kgdboc/parameters/kgdboc
```

## 启动
```shell
echo g > /proc/sysrq-trigger

<!-- 注意: -->
<!-- 系统 panic 也会触发 kdb 调试 -->
echo c > /proc/sysrq-trigger
```
