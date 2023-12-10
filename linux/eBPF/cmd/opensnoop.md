# opensnoop

跟踪所有 open() 系统调用

```shell
sudo /usr/share/bcc/tools/opensnoop

PID    COMM               FD ERR PATH
1      systemd            35   0 /proc/self/mountinfo
2797   udisksd            13   0 /proc/self/mountinfo
1      systemd            35   0 /sys/devices/pci0000:00/0000:00:0d.0/ata3/host2/target2:0:0/2:0:0:0/block/sda/sda1/uevent
1      systemd            35   0 /run/udev/data/b8:1
1      systemd            -1   2 /etc/systemd/system/sys-kernel-debug-tracing.mount
1      systemd            -1   2 /run/systemd/system/sys-kernel-debug-tracing.mount
1      systemd            -1   2 /run/systemd/generator/sys-kernel-debug-tracing.mount
1      systemd            -1   2 /usr/local/lib/systemd/system/sys-kernel-debug-tracing.mount
2247   systemd            15   0 /proc/self/mountinfo
1      systemd            -1   2 /lib/systemd/system/sys-kernel-debug-tracing.mount
1      systemd            -1   2 /usr/lib/systemd/system/sys-kernel-debug-tracing.mount
1      systemd            -1   2 /run/systemd/generator.late/sys-kernel-debug-tracing.mount
1      systemd            -1   2 /etc/systemd/system/sys-kernel-debug-tracing.mount.wants
1      systemd            -1   2 /etc/systemd/system/sys-kernel-debug-tracing.mount.requires
1      systemd            -1   2 /run/systemd/system/sys-kernel-debug-tracing.mount.wants
1      systemd            -1   2 /run/systemd/system/sys-kernel-debug-tracing.mount.requires
1      systemd            -1   2 /run/systemd/generator/sys-kernel-debug-tracing.mount.wants
1      systemd            -1   2 /run/systemd/generator/sys-kernel-debug-tracing.mount.requires
1      systemd            -1   2 /usr/local/lib/systemd/system/sys-kernel-debug-tracing.mount.wants
1      systemd            -1   2 /usr/local/lib/systemd/system/sys-kernel-debug-tracing.mount.requires
1      systemd            -1   2 /lib/systemd/system/sys-kernel-debug-tracing.mount.wants
1      systemd            -1   2 /lib/systemd/system/sys-kernel-debug-tracing.mount.requires
1      systemd            -1   2 /usr/lib/systemd/system/sys-kernel-debug-tracing.mount.wants
1      systemd            -1   2 /usr/lib/systemd/system/sys-kernel-debug-tracing.mount.requires
1      systemd            -1   2 /run/systemd/generator.late/sys-kernel-debug-tracing.mount.wants
1      systemd            -1   2 /run/systemd/generator.late/sys-kernel-debug-tracing.mount.requires
1      systemd            -1   2 /etc/systemd/system/sys-kernel-debug-tracing.mount.d
1      systemd            -1   2 /run/systemd/system/sys-kernel-debug-tracing.mount.d
1      systemd            -1   2 /run/systemd/generator/sys-kernel-debug-tracing.mount.d
```
