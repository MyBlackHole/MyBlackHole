# modules
[[modprobe]]

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


