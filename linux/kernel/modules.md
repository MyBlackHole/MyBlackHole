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


