# Sar
linux 性能监控
  sar命令可以从文件的读写情况、系统调用的使用情况、磁盘I/O、CPU效率、内存使用状况、进程活动及IPC有关的活动等方面进行报告。     sar命令是Linux下系统运行状态统计工具，它将指定的操作系统状态计数器显示到标准输出设备。sar工具将对系统当前的状态进行取样，然后通过计算数据和比例来表达系统的当前运行状态。它的特点是可以连续对系统取样，获得大量的取样数据。取样数据和分析的结果都可以存入文件，使用它时消耗的系统资源很小。 

安装sar命令 

## install

yum  install sysstat 

## 语法 

sar(选项)(参数) 
sar [options] [-A] [-o file] t [n] 
其中： options 为命令行选项，t为采样间隔，n为采样次数，默认值是1； -o file表示将命令结果以二进制格式存放在文件中，file 是文件名。 

### 选项 

-A：显示所有的报告信息；
-b：显示I/O速率； 
-B：显示换页状态； 
-c：显示进程创建活动；
-d：显示每个块设备的状态；
-e：设置显示报告的结束时间；
-f：从指定文件提取报告； 
-i：设状态信息刷新的间隔时间； 
-P：报告每个CPU的状态；
-R：显示内存状态；
-u：显示CPU利用率；
-v：显示索引节点，文件和其他内核表的状态； 
-w：显示交换分区状态； 
-x：显示给定进程的状态。 

### 参数 

间隔时间：每次报告的间隔时间（秒）； 次数：显示报告的次数。 

## 实例 

### CPU资源监控： 
每5s采样一次，连续采样3次，观察cpu使用情况，并将采样结果以二进制形式存入test_sar中（查看二进制文件test中的内容，sar命令：sar -u -f test_sar） 

```shell
sar -u -o test_sar 5 3 Linux 3.10.0-693.17.1.el7.x86_64 (zabbix_nuc)   2018年03月29日     _x86_64_    (4 CPU)  10时28分24秒     CPU     %user     %nice   %system   %iowait    %steal     %idle 10时28分29秒     all      0.20      0.00      0.10      0.00      0.00     99.70 10时28分34秒     all      0.10      0.00      0.20      0.00      0.00     99.70 10时28分39秒     all      0.20      0.00      0.10      0.00      0.00     99.70 平均时间:        all      0.17      0.00      0.13      0.00      0.00     99.70
```
输出项说明： 
CPU：all表示统计信息为所有 CPU的平均值。  %user：显示在用户级别(application)运行使用 CPU总时间的百分比。  %nice：显示在用户级别，用于nice操作，所占用 CPU总时间的百分比。  %system：在核心级别(kernel)运行所使用 CPU总时间的百分比。  %iowait：显示用于等待I/O操作占用 CPU总时间的百分比。  %steal：管理程序(hypervisor)为另一个虚拟进程提供服务而等待虚拟CPU 的百分比。  %idle：显示 CPU空闲时间占用 CPU总时间的百分比。 

P.S： 
1. 若 %iowait的值过高，表示硬盘存在I/O瓶颈  2.若 %idle的值高但系统响应慢时，有可能是CPU等待分配内存，此时应加大内存容量  3.若 %idle的值持续低于1，则系统的CPU处理能力相对较低，表明系统中最需要解决的资源是 CPU。 
2. inode、文件和其他内核表监控： 

### 每5秒采样一次，连续采样10次，观察核心表的状态 
```shell
sar -v 5 10 Linux 3.10.0-693.17.1.el7.x86_64 (zabbix_nuc)   2018年03月29日     _x86_64_    (4 CPU)  10时32分43秒 dentunusd   file-nr  inode-nr    pty-nr 10时32分48秒    226121      1664     86294         1 10时32分53秒    226121      1664     86294         1 10时32分58秒    226121      1664     86294         1 10时33分03秒    226118      1664     86290         1 10时33分08秒    226118      1664     86291         1 10时33分13秒    226126      1664     86300         1 10时33分18秒    226126      1664     86298         1 10时33分23秒    226126      1632     86298         1 10时33分28秒    226126      1664     86299         1 10时33分33秒    226126      1664     86298         1 平均时间:       226123      1661     86296         1
```
输出项说明： 

dentunusd：目录高速缓存中未被使用的条目数量  file-nr：文件句柄（filehandle）的使用数量  inode-nr：索引节点句柄（inodehandle）的使用数量  pty-nr：使用的pty数量 

### 内存和交换空间监控： 

每5s采样一次，连续采样10次，监控内存分页 
```shell
sar -r 5 10 Linux 3.10.0-693.17.1.el7.x86_64 (zabbix_nuc)   2018年03月29日     _x86_64_    (4 CPU)  10时35分23秒 kbmemfree kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty 10时35分28秒  12430300   3720688     23.04      2288   2536368   2274384      8.54   2861256    439388        20 10时35分33秒  12430636   3720352     23.03      2288   2536368   2274384      8.54   2861288    439388         8 10时35分38秒  12430068   3720920     23.04      2288   2536368   2274384      8.54   2861288    439388         8 10时35分43秒  12430068   3720920     23.04      2288   2536368   2274384      8.54   2861288    439388        12 10时35分48秒  12430440   3720548     23.04      2288   2536368   2274384      8.54   2861288    439388        12 10时35分53秒  12430440   3720548     23.04      2288   2536368   2274384      8.54   2861288    439388        20 10时35分58秒  12430440   3720548     23.04      2288   2536368   2274384      8.54   2861288    439388         8 10时36分03秒  12430440   3720548     23.04      2288   2536368   2274384      8.54   2861288    439388         8 10时36分08秒  12430440   3720548     23.04      2288   2536368   2274384      8.54   2861288    439388         8 10时36分13秒  12430408   3720580     23.04      2288   2536368   2274384      8.54   2861288    439388        20 平均时间:     12430368   3720620     23.04      2288   2536368   2274384      8.54   2861285    439388        12
```
输出项说明： 

kbmemfree：这个值和free命令中的free值基本一致,所以它不包括buffer和cache的空间.  kbmemused：这个值和free命令中的used值基本一致,所以它包括buffer和cache的空间.  %memused：这个值是kbmemused和内存总量(不包括swap)的一个百分比.  kbbuffers和kbcached：这两个值就是free命令中的buffer和cache.  kbcommit：保证当前系统所需要的内存,即为了确保不溢出而需要的内存(RAM+swap).  %commit：这个值是kbcommit与内存总量(包括swap)的一个百分比. 

### 内存分页监控： 
每10s采样一次，连续采样3次，监控内存分页 
```shell
sar -B 10 3 Linux 3.10.0-693.17.1.el7.x86_64 (zabbix_nuc)   2018年03月29日     _x86_64_    (4 CPU)  12时08分10秒  pgpgin/s pgpgout/s   fault/s  majflt/s  pgfree/s pgscank/s pgscand/s pgsteal/s    %vmeff 12时08分20秒      0.00    754.40     26.20      0.00     35.90      0.00      0.00      0.00      0.00 12时08分30秒      0.00    429.60    172.00      0.00     87.00      0.00      0.00      0.00      0.00 12时08分40秒      0.00    272.40    221.60      0.00     95.60      0.00      0.00      0.00      0.00 平均时间:      0.00    485.47    139.93      0.00     72.83      0.00      0.00      0.00      0.00
```

输出项说明： 
pgpgin/s：表示每秒从磁盘或SWAP置换到内存的字节数(KB)  pgpgout/s：表示每秒从内存置换到磁盘或SWAP的字节数(KB)  fault/s：每秒钟系统产生的缺页数,即主缺页与次缺页之和(major +minor)  majflt/s：每秒钟产生的主缺页数.  pgfree/s：每秒被放入空闲队列中的页个数  pgscank/s：每秒被kswapd扫描的页个数  pgscand/s：每秒直接被扫描的页个数  pgsteal/s：每秒钟从cache中被清除来满足内存需要的页个数  %vmeff：每秒清除的页(pgsteal)占总扫描页(pgscank+pgscand)的百分比 

### I/O和传送速率监控： 

每5s采样一次，连续采样10次，报告缓冲区的使用情况 
```shell
sar -b 5 10 Linux 3.10.0-693.17.1.el7.x86_64 (zabbix_nuc)   2018年03月29日     _x86_64_    (4 CPU)  10时40分18秒       tps      rtps      wtps   bread/s   bwrtn/s 10时40分23秒      3.00      0.00      3.00      0.00     28.80 10时40分28秒      4.60      0.00      4.60      0.00     64.00 10时40分33秒      3.20      0.00      3.20      0.00     33.60 10时40分38秒      3.20      0.00      3.20      0.00     36.80 10时40分43秒      2.40      0.00      2.40      0.00     24.00 10时40分48秒      3.60      0.00      3.60      0.00     35.20 10时40分53秒      2.80      0.00      2.80      0.00     32.00 10时40分58秒      3.40      0.00      3.40      0.00     32.00 10时41分03秒      5.60      0.00      5.60      0.00     91.20 10时41分08秒      3.40      0.00      3.40      0.00     35.20 平均时间:         3.52      0.00      3.52      0.00     41.28
```

输出项说明： 
tps：每秒钟物理设备的 I/O传输总量  rtps：每秒钟从物理设备读入的数据总量  wtps：每秒钟向物理设备写入的数据总量  bread/s：每秒钟从物理设备读入的数据量，单位为块/s  bwrtn/s：每秒钟向物理设备写入的数据量，单位为块/s 

### 进程队列长度和平均负载状态监控： 

每5s采样一次，连续采样10次，监控进程队列长度和平均负载状态 
```shell
sar -q 5 10 Linux 3.10.0-693.17.1.el7.x86_64 (zabbix_nuc)   2018年03月29日     _x86_64_    (4 CPU)  10时42分34秒   runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked 10时42分39秒         0       273      0.06      0.18      0.40         0 10时42分44秒         0       273      0.06      0.18      0.40         0 10时42分49秒         0       273      0.05      0.17      0.40         0 10时42分54秒         0       273      0.05      0.17      0.40         0 10时42分59秒         1       274      0.05      0.17      0.39         0 10时43分04秒         0       274      0.04      0.17      0.39         0 10时43分09秒         0       274      0.04      0.16      0.39         0 10时43分14秒         0       274      0.04      0.16      0.39         0 10时43分19秒         0       272      0.03      0.16      0.39         0 10时43分24秒         0       273      0.03      0.16      0.39         0 平均时间:            0       273      0.04      0.17      0.39         0
```

输出项说明： 

runq-sz：运行队列的长度（等待运行的进程数）  plist-sz：进程列表中进程（processes）和线程（threads）的数量  ldavg-1：最后1分钟的系统平均负载（Systemload average）  ldavg-5：过去5分钟的系统平均负载  ldavg-15：过去15分钟的系统平均负载 

### 系统交换活动信息监控： 

每5s采样一次，连续采样10次，监控系统交换活动信息 
```shell
sar -W 5 10 Linux 3.10.0-693.17.1.el7.x86_64 (zabbix_nuc)   2018年03月29日     _x86_64_    (4 CPU)  10时44分20秒  pswpin/s pswpout/s 10时44分25秒      0.00      0.00 10时44分30秒      0.00      0.00 10时44分35秒      0.00      0.00 10时44分40秒      0.00      0.00 10时44分45秒      0.00      0.00 10时44分50秒      0.00      0.00 10时44分55秒      0.00      0.00 10时45分00秒      0.00      0.00 10时45分05秒      0.00      0.00 10时45分10秒      0.00      0.00 平均时间:         0.00      0.00
```

输出项说明： 

pswpin/s：每秒系统换入的交换页面（swap page）数量  pswpout/s：每秒系统换出的交换页面（swap page）数量 

### 统计网络信息 

#sar -n选项使用6个不同的开关：DEV，EDEV，NFS，NFSD，SOCK，IP，EIP，ICMP，EICMP，TCP，ETCP，UDP，SOCK6，IP6，EIP6，ICMP6，EICMP6和UDP6 ，DEV显示网络接口信息，EDEV显示关于网络错误的统计数据，NFS统计活动的NFS客户端的信息，NFSD统计NFS服务器的信息，SOCK显示套接字信息，ALL显示所有5个开关。它们可以单独或者一起使用。 

#### sar -n DEV 1 3 

每间隔1秒统计一次，总计统计3次，下面的average是在多次统计后的平均值 
```shell
sar -n DEV 1 3 Linux 3.10.0-693.17.1.el7.x86_64 (zabbix_nuc)   2018年03月29日     _x86_64_    (4 CPU)  11时03分48秒     IFACE   rxpck/s   txpck/s    rxkB/s    txkB/s   rxcmp/s   txcmp/s  rxmcst/s 11时03分49秒   enp0s25    103.00    108.00      6.84      7.25      0.00      0.00      0.00 11时03分49秒    wlp2s0      0.00      0.00      0.00      0.00      0.00      0.00      0.00 11时03分49秒        lo      0.00      0.00      0.00      0.00      0.00      0.00      0.00  11时03分49秒     IFACE   rxpck/s   txpck/s    rxkB/s    txkB/s   rxcmp/s   txcmp/s  rxmcst/s 11时03分50秒   enp0s25     86.00     94.00      5.72      6.85      0.00      0.00      0.00 11时03分50秒    wlp2s0      0.00      0.00      0.00      0.00      0.00      0.00      0.00 11时03分50秒        lo      0.00      0.00      0.00      0.00      0.00      0.00      0.00  11时03分50秒     IFACE   rxpck/s   txpck/s    rxkB/s    txkB/s   rxcmp/s   txcmp/s  rxmcst/s 11时03分51秒   enp0s25    105.00    112.00      6.95      8.01      0.00      0.00      0.00 11时03分51秒    wlp2s0      0.00      0.00      0.00      0.00      0.00      0.00      0.00 11时03分51秒        lo      0.00      0.00      0.00      0.00      0.00      0.00      0.00  平均时间:     IFACE   rxpck/s   txpck/s    rxkB/s    txkB/s   rxcmp/s   txcmp/s  rxmcst/s 平均时间:   enp0s25     98.00    104.67      6.50      7.37      0.00      0.00      0.00 平均时间:    wlp2s0      0.00      0.00      0.00      0.00      0.00      0.00      0.00 平均时间:        lo      0.00      0.00      0.00      0.00      0.00      0.00      0.00
```

输出项说明： 

#IFACE 本地网卡接口的名称  #rxpck/s 每秒钟接受的数据包  #txpck/s 每秒钟发送的数据库  #rxKB/S 每秒钟接受的数据包大小，单位为KB  #txKB/S 每秒钟发送的数据包大小，单位为KB  #rxcmp/s 每秒钟接受的压缩数据包  #txcmp/s 每秒钟发送的压缩包  #rxmcst/s 每秒钟接收的多播数据包 

#### sar -n EDEV 1 1 
统计网络设备通信失败信息
```shell
sar -n EDEV  1 1 Linux 3.10.0-693.17.1.el7.x86_64 (zabbix_nuc)   2018年03月29日     _x86_64_    (4 CPU)  11时07分21秒     IFACE   rxerr/s   txerr/s    coll/s  rxdrop/s  txdrop/s  txcarr/s  rxfram/s  rxfifo/s  txfifo/s 11时07分22秒   enp0s25      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00 11时07分22秒    wlp2s0      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00 11时07分22秒        lo      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00  平均时间:     IFACE   rxerr/s   txerr/s    coll/s  rxdrop/s  txdrop/s  txcarr/s  rxfram/s  rxfifo/s  txfifo/s 平均时间:   enp0s25      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00 平均时间:    wlp2s0      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00 平均时间:        lo      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
```

输出项说明： 

#IFACE 网卡名称  #rxerr/s 每秒钟接收到的损坏的数据包  #txerr/s 每秒钟发送的数据包错误数  #coll/s 当发送数据包时候，每秒钟发生的冲撞（collisions）数，这个是在半双工模式下才有  #rxdrop/s 当由于缓冲区满的时候，网卡设备接收端每秒钟丢掉的网络包的数目  #txdrop/s 当由于缓冲区满的时候，网络设备发送端每秒钟丢掉的网络包的数目  #txcarr/s  当发送数据包的时候，每秒钟载波错误发生的次数  #rxfram   在接收数据包的时候，每秒钟发生的帧对其错误的次数  #rxfifo    在接收数据包的时候，每秒钟缓冲区溢出的错误发生的次数  #txfifo    在发生数据包的时候，每秒钟缓冲区溢出的错误发生的次数 

#### sar -n SOCK 1 1 
统计socket连接信息 
```shell
sar -n SOCK 1 1 Linux 3.10.0-693.17.1.el7.x86_64 (zabbix_nuc)   2018年03月29日     _x86_64_    (4 CPU)  11时09分42秒    totsck    tcpsck    udpsck    rawsck   ip-frag    tcp-tw 11时09分43秒       253         6         0         0         0       196 平均时间:          253         6         0         0         0       196
```

输出项说明： 

#totsck 当前被使用的socket总数  #tcpsck 当前正在被使用的TCP的socket总数  #udpsck  当前正在被使用的UDP的socket总数  #rawsck 当前正在被使用于RAW的skcket总数  #if-frag  当前的IP分片的数目  #tcp-tw TCP套接字中处于TIME-WAIT状态的连接数量  如果你使用FULL关键字，相当于上述DEV、EDEV和SOCK三者的综合 

#### sar -n TCP 1 3 
TCP连接的统计 
```shell
sar -n TCP 1 3 Linux 3.10.0-693.17.1.el7.x86_64 (zabbix_nuc)   2018年03月29日     _x86_64_    (4 CPU)  11时11分28秒  active/s passive/s    iseg/s    oseg/s 11时11分29秒     25.00      2.00    134.00    138.00 11时11分30秒     29.00      2.00    153.00    159.00 11时11分31秒     27.00      2.00    143.00    149.00 平均时间:        27.00      2.00    143.33    148.67
```

输出项说明： 

#active/s 新的主动连接  #passive/s 新的被动连接  #iseg/s 接受的段  #oseg/s 输出的段    1.10.5：sar -n 使用总结 -n DEV ： 网络接口统计信息。 -n EDEV ： 网络接口错误。 -n IP ： IP数据报统计信息。 -n EIP ： IP错误统计信息。 -n TCP ： TCP统计信息。 -n ETCP ： TCP错误统计信息。 -n SOCK ： 套接字使用。 

### 设备使用情况监控： 

每5s采样一次，连续采样10次，报告设备使用情况 

```shell
sar -d 5 10 -p Linux 3.10.0-693.17.1.el7.x86_64 (zabbix_nuc)   2018年03月29日     _x86_64_    (4 CPU)  10时46分29秒       DEV       tps  rd_sec/s  wr_sec/s  avgrq-sz  avgqu-sz     await     svctm     %util 10时46分34秒       sda     13.60      0.00    486.40     35.76      0.29     21.43     18.01     24.50  10时46分34秒       DEV       tps  rd_sec/s  wr_sec/s  avgrq-sz  avgqu-sz     await     svctm     %util 10时46分39秒       sda     13.80      0.00    468.80     33.97      0.25     17.75     17.49     24.14  10时46分39秒       DEV       tps  rd_sec/s  wr_sec/s  avgrq-sz  avgqu-sz     await     svctm     %util 10时46分44秒       sda     47.60      0.00   3000.00     63.03      1.68     35.35     14.05     66.88  10时46分44秒       DEV       tps  rd_sec/s  wr_sec/s  avgrq-sz  avgqu-sz     await     svctm     %util 10时46分49秒       sda     36.40      0.00   2750.40     75.56      4.68    128.58     21.33     77.64  10时46分49秒       DEV       tps  rd_sec/s  wr_sec/s  avgrq-sz  avgqu-sz     await     svctm     %util 10时46分54秒       sda      7.80      0.00    177.60     22.77      0.74     60.08     66.23     51.66  10时46分54秒       DEV       tps  rd_sec/s  wr_sec/s  avgrq-sz  avgqu-sz     await     svctm     %util 10时46分59秒       sda      7.80      0.00    132.80     17.03      0.35     78.56     32.41     25.28  10时46分59秒       DEV       tps  rd_sec/s  wr_sec/s  avgrq-sz  avgqu-sz     await     svctm     %util 10时47分04秒       sda      8.80      0.00    177.60     20.18      0.33     37.45     24.84     21.86  10时47分04秒       DEV       tps  rd_sec/s  wr_sec/s  avgrq-sz  avgqu-sz     await     svctm     %util 10时47分09秒       sda      6.00      0.00    136.00     22.67      0.76    127.10     77.53     46.52  10时47分09秒       DEV       tps  rd_sec/s  wr_sec/s  avgrq-sz  avgqu-sz     await     svctm     %util 10时47分14秒       sda      6.20      0.00    129.60     20.90      0.80    125.74     80.55     49.94  10时47分14秒       DEV       tps  rd_sec/s  wr_sec/s  avgrq-sz  avgqu-sz     await     svctm     %util 10时47分19秒       sda      6.60      0.00    155.20     23.52      0.50     78.58     53.70     35.44  平均时间:          DEV       tps  rd_sec/s  wr_sec/s  avgrq-sz  avgqu-sz     await     svctm     %util 平均时间:          sda     15.46      0.00    761.44     49.25      1.04     67.08     27.42     42.39
```

参数 

-p可以打印出sda,hdc等磁盘设备名称,如果不用参数-p,设备节点则有可能是dev8-0,dev22-0 

输出项说明： 

tps:每秒从物理磁盘I/O的次数.多个逻辑请求会被合并为一个I/O磁盘请求,一次传输的大小是不确定的.  rd_sec/s:每秒读扇区的次数.  wr_sec/s:每秒写扇区的次数.  avgrq-sz:平均每次设备I/O操作的数据大小(扇区).  avgqu-sz:磁盘请求队列的平均长度.  await:从请求磁盘操作到系统完成处理,每次请求的平均消耗时间,包括请求队列等待时间,单位是毫秒(1秒=1000毫秒).  svctm:系统处理每次请求的平均时间,不包括在请求队列中消耗的时间.  %util:I/O请求占CPU的百分比,比率越大,说明越饱和.  1. avgqu-sz的值较低时，设备的利用率较高。  2.当%util的值接近 1%时，表示设备带宽已经占满 

总结： 

要判断系统瓶颈问题，有时需几个 sar 命令选项结合起来  怀疑CPU存在瓶颈，可用 sar -u和 sar -q 等来查看  怀疑内存存在瓶颈，可用 sar -B、sar -r和 sar -W 等来查看  怀疑I/O存在瓶颈，可用 sar -b、sar -u 和 sar -d等来查看 

常用命令汇总： 

因版本和平台不同，有部分命令可能没有或显示结果不一致：  
默认监控: sar 5 5     //  CPU和IOWAIT统计状态 
(1) sar -b 5 5        // IO传送速率
(2) sar -B 5 5        // 页交换速率 
(3) sar -c 5 5        // 进程创建的速率
(4) sar -d 5 5        // 块设备的活跃信息 
(5) sar -n DEV 5 5    // 网路设备的状态信息
(6) sar -n SOCK 5 5   // SOCK的使用情况 
(7) sar -n ALL 5 5    // 所有的网络状态信息
(8) sar -P ALL 5 5    // 每颗CPU的使用状态信息和IOWAIT统计状态
(9) sar -q 5 5        // 队列的长度（等待运行的进程数）和负载的状态 
(10) sar -r 5 5       // 内存和swap空间使用情况
(11) sar -R 5 5       //     内存的统计信息（内存页的分配和释放、系统每秒作为BUFFER使用内存页、每秒被cache到的内存页）
(12) sar -u 5 5       // CPU的使用情况和IOWAIT信息（同默认监控）
(13) sar -v 5 5       // inode, file and other kernel tablesd的状态信息
(14) sar -w 5 5       // 每秒上下文交换的数目
(15) sar -W 5 5       // SWAP交换的统计信息(监控状态同iostat 的si so)
(16) sar -x 2906 5 5  // 显示指定进程(2906)的统计信息，信息包括：进程造成的错误、用户级和系统级用户CPU的占用情况、运行在哪颗CPU上
(17) sar -y 5 5       // TTY设备的活动状态
(18) 将输出到文件(-o)和读取记录信息(-f)   sar也可以监控非实时数据，通过cron周期的运行到指定目录下 例如:我们想查看本月27日,从0点到23点的内存资源. sa27就是本月27日,指定具体的时间可以通过-s(start)和-e(end)来指定. sar -f /var/log/sa/sa27 -s 00:00:00 -e 23:00:00 -r