# kasan

Kernel Address Sanitizer


- bclinux 配置
```shell

yum install bison-devel ncurses-devel flex bc


CONFIG_KASAN=y
CONFIG_KASAN_INLINE=y
# CONFIG_KASAN_OUTLINE=y
CONFIG_SLUB_DEBUG=y
CONFIG_SLUB_DEBUG_ON=y

```
