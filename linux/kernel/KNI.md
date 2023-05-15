# KNI
KNI（Kernel NIC Interface，内核网卡接口），是 DPDK 允许用户态和内核态交换报文的解决方案
模拟了一个虚拟的网口，提供 DPDK 应用程序和 Linux 内核之间通讯没接
即 KNI 接口允许报文从用户态接收后转发到 Linux 内核协议栈中去
