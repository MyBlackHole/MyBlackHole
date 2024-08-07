# kexec

[来源](https://qkxu.github.io/2019/06/05/CentOS-7-%E5%86%85%E6%A0%B8%E8%B0%83%E8%AF%95%E5%B7%A5%E5%85%B7.html)
在调试过程中，经常需要重启内核以还原现场，进而复现某些问题予以追踪解决。
由于每一次的内核启动，都会伴随着一次的boot自检。
但是，对于已经启动过的同一内核，重复的boot自检完全没有必要，且造成了资源浪费。
此外，有时候我们需要使用一个小内核来启动一个大内核。
在这两种需求下，kexec应运而生，kexec是一款可以让您重新启动到一个新 Linux 内核的快速重新引导功能部件，
不再必须通过固件和引导装载程序阶段，从而跳过序列中最长的部分，大大减少了重启时间。
对企业级系统而言，Kexec 大大减少了重启引起的系统宕机时间。
对内核和系统软件开发者而言，Kexec 帮助您在开发和测试成果时可以迅速重新启动系统，而不必每次都要再经历耗时的固件阶段

```shell
yum install kexec-tools

需要检测本系统内核是否已经选中支持kexec system call，
若在“/boot/config-XXXXX”中CONFIG_KEXEC=y则是本版本号的内核已开启；
若=n，则需要重新编译内核重置CONFIG_KEXEC=y
```
