# modules
[[modprobe]]


## config
```shell
obj-m:= hello.o
CURRENT_PATH:=$(shell pwd)
LINUX_KERNEL:=$(shell uname -r)
LINUX_KERNEL_PATH:=/lib/modules/$(LINUX_KERNEL)/build

all:
        make -C $(LINUX_KERNEL_PATH) M=$(CURRENT_PATH) modules
clean:
        make -C $(LINUX_KERNEL_PATH) M=$(CURRENT_PATH) clean
```


## 例子
- 列表目录
```shell
/lib/modules/
/usr/lib/modules
```

- 当前载入列表
```shell
cat /proc/modules
```

```shell
<!-- centos -->
yum install kernel-devel

/lib/modules/$(shell uname -r)/build
```


- 编译携带调试信息的模块
```shell
EXTRA_CFLAGS= -O0
-------------------

EXTRA_CFLAGS = -g

all:
	make -C $(KDIR) M=$(PWD) ARCH=arm CROSS_COMPILE=$(CROSS_COMPILE) modules
```


- 指定内核源码路径 交叉编译
```shell
KDIR=/run/media/black/Data/Documents/linux_debug/arm/linux-4.19.315 make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu-
```
