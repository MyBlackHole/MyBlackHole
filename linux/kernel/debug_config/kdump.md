# kdump

[来源](https://qkxu.github.io/2019/06/05/CentOS-7-%E5%86%85%E6%A0%B8%E8%B0%83%E8%AF%95%E5%B7%A5%E5%85%B7.html)

kdump是内核转储工具，kexec是实现kdump机制的关键。kdump是基于kexec的内核崩溃转储机制。

当系统崩溃时，kdump利用kexec启动另一个内核，这一个内核叫捕获内核，以很小的内存启动捕获内核，第一个内核保留了内存的一部分给捕获内核启动用。kdump利用kexec启动捕获内核，免去启动BIOS(绕过BIOS，没有经历BIOS，节省了系统启动的时间)，所以第一个内核的内存得以保留。

捕获内核只会使用分配给他的内存空间，不会污染第一内核的内存数据。

```shell
yum install kexec-tools

systemctl start kdump
systemctl enable kdump
```

- config
```shell
配置内核启动参数:

必须在引导“第一内核”期间为捕获内核保留 crashkernel 的内存，
一般设置为 crashkernel=auto 表示根据系统内存自动 reserve 一些内存给 kernelcrash 用。

vim /boot/grub2/grub.cfg
...
linux16 /vmlinuz-3.10.0-693.21.1.el7.x86_64 root=/dev/mapper/centos-root ro crashkernel=auto ...
...


kdump.service 相关的配置文件 /etc/kdump.conf 里面可以修改一些默认的配置，
比如dump完成后的动作（默认是reboot）、dump文件存放的方式（本地目录、NFS、scp到另外服务器等），默认存在本地目录:

path /var/crash
```
