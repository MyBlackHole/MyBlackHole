- value optimized out
```shell
# 在内核源码的Makefile文件中，找到CONFIG_CC_OPTIMIZE_FOR_SIZE，可以看到如下代码
ifdef CONFIG_CC_OPTIMIZE_FOR_SIZE
KBUILD_CFLAGS   += -Os $(call cc-disable-warning,maybe-uninitialized,)
else
KBUILD_CFLAGS   += -O2
endif

# 如果你打开了CONFIG_CC_OPTIMIZE_FOR_SIZE
# 就把那里的-Os改为-O0，
# 如果关闭了CONFIG_CC_OPTIMIZE_FOR_SIZE，就把下面的-O2改为-O0试试。
# 
# 编译时指定-O0：不进行优化
```

- /dev/net/tun: Operation not permitted
```shell
sudo modprobe tun
```
