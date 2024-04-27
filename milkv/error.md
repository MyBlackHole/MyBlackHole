# error

- milkv 启动堵塞
[    2.802087] sync_task_init:177(): sync_task_init vi_pipe 2
[    2.808693] vi_core_probe:252(): isp registered as cvi-vi
[    2.865589] cvi_dwa_probe:487(): done with rc(0).
```shell
<!-- 看配置 -->
<!-- 一步步定位 -->

# insmod /mnt/system/ko/cv180x_thermal.ko

最终还是没有用的
```

- not rndis
```shell
# archlinux 可能未启用 usbfunc:rndis 模块
sudo modprobe usbfunc:rndis
```
