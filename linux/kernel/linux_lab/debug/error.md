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
<!-- 内核 O0无法编译通过 -->

<!-- 以下待验证 -->
1. 修改对应内核源码中的Makefile (到openwrt生成的build_dir中找)
    将KBUILD_CFLAGS变量中的-O2改成-O1,让编译只进行简单的优化

2. 使能内核的编译选项CONFIG_DEBUG_SECTION_MISMATCH,防止内联 (可选)
　　make kernel_menuconfig (在menuconfig中使能选项CONFIG_DEBUG_SECTION_MISMATCH)
　　Symbol: DEBUG_SECTION_MISMATCH [=y] 
　　Type: boolean
        Prompt: Enable full Section mismatch analysis
　　Location: 
　　-> Kernel hacking 
　　　　-> Compile-time checks and compiler options
```

- /dev/net/tun: Operation not permitted
```shell
sudo modprobe tun
```
