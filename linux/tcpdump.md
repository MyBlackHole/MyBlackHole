# tcpdump
## 语法
```shell
tcpdump [ -AdDefIJKlLnNOpqRStuUvxX ] [ -B buffer_size ] [ -c count ]
               [ -C file_size ] [ -G rotate_seconds ] [ -F file ]
               [ -i interface ] [ -j tstamp_type ] [ -m module ] [ -M secret ]
               [ -Q|-P in|out|inout ]
               [ -r file ] [ -s snaplen ] [ -T type ] [ -w file ]
               [ -W filecount ]
               [ -E spi@ipaddr algo:secret,...  ]
               [ -y datalinktype ] [ -z postrotate-command ] [ -Z user ]
               [ expression ]
```

### 参数
```shell
-A 以ASCII格式打印出所有分组，并将链路层的头最小化。
-c 在收到指定的数量的分组后，tcpdump就会停止。
-C 在将一个原始分组写入文件之前，检查文件当前的大小是否超过了参数file_size
中指定的大小。如果超过了指定大小，则关闭当前文件，然后在打开一个新的文件。参数 file_size
的单位是兆字节（是1,000,000字节，而不是1,048,576字节）。
-d 将匹配信息包的代码以人们能够理解的汇编格式给出。
-dd 将匹配信息包的代码以c语言程序段的格式给出。
-ddd 将匹配信息包的代码以十进制的形式给出。
-D 打印出系统中所有可以用tcpdump截包的网络接口。
-e 在输出行打印出数据链路层的头部信息。
-E 用spi@ipaddr algo:secret解密那些以addr作为地址，并且包含了安全参数索引值spi的IPsec ESP分组。
-f 将外部的Internet地址以数字的形式打印出来。
-F 从指定的文件中读取表达式，忽略命令行中给出的表达式。
-i 指定监听的网络接口。
-l 使标准输出变为缓冲行形式。
-L 列出网络接口的已知数据链路。
-m 从文件module中导入SMI MIB模块定义。该参数可以被使用多次，以导入多个MIB模块。
-M 如果tcp报文中存在TCP-MD5选项，则需要用secret作为共享的验证码用于验证TCP-MD5选选项摘要（详情可参考RFC 2385）。
-n 不把网络地址转换成名字。
-N 不输出主机名中的域名部分。例如，link.linux265.com 只输出link。
-t 在输出的每一行不打印时间戳。
-O 不运行分组分组匹配（packet-matching）代码优化程序。
-P 不将网络接口设置成混杂模式。
-q 快速输出。只输出较少的协议信息。
-r 从指定的文件中读取包(这些包一般通过-w选项产生)。
-S 将tcp的序列号以绝对值形式输出，而不是相对值。
-s 从每个分组中读取最开始的snaplen个字节，而不是默认的68个字节。
-T 将监听到的包直接解释为指定的类型的报文，常见的类型有rpc远程过程调用）和snmp（简单网络管理协议；）。
-t 不在每一行中输出时间戳。
-tt 在每一行中输出非格式化的时间戳。
-ttt 输出本行和前面一行之间的时间差。
-tttt 在每一行中输出由date处理的默认格式的时间戳。
-u 输出未解码的NFS句柄。
-v 输出一个稍微详细的信息，例如在ip包中可以包括ttl和服务类型的信息。
-vv 输出详细的报文信息。
-w 直接将分组写入文件中，而不是不分析并打印出来。
-x 以16进制数形式显示每一个报文 (去掉链路层报头) . 可以显示较小的完整报文, 否则只显示snaplen个字节.
-xx 以16进制数形式显示每一个报文（包含链路层包头）。
-X 以16进制和ASCII码形式显示每个报文（去掉链路层报头）。
-XX 以16进制和ASCII吗形式显示每个报文（包含链路层报头）。
-y 设置tcpdump 捕获数据链路层协议类型
-Z 使tcpdump 放弃自己的超级权限(如果以root用户启动tcpdump, tcpdump将会有超级用户权限), 并把当前tcpdump的用户ID设置为user, 组ID设置为user首要所属组的ID
```

## 例子

- 抓取所有网络包
```shell
tcpdump
```

- 抓取所有的网络包, 并存到 tcpdump.cap 文件中
```shell
tcpdump -w tcpdump.cap
```
- 抓取所有的经过 eth0 网卡的网络包, 存到tcpdump.cap 文件中
```shell
tcpdump -i eth0 -w tcpdump.cap
```

- 抓取源地址是 192.168.1.100 的包, 将结果保存到 tcpdump.cap 文件中。
```shell
tcpdump src host 192.168.1.100 -w tcpdump.cap
```

- 抓取地址包含是 192.168.1.100 的包，将结果保存到 tcpdump.cap 文件中。
```shell
tcpdump host 192.168.1.100 -w tcpdump.cap
```

- 抓取目的地址包含是 192.168.1.100 的包，将结果保存到 tcpdump.cap 文件中。
```shell
tcpdump dest host 192.168.1.100 -w tcpdump.cap
```
- 抓取主机地址为 192.168.1.100 的数据包
```shell
tcpdump -i eth0 -vnn host 192.168.1.100
```
- 抓取包含 192.168.1.0/24 网段的数据包
```shell
tcpdump -i eth0 -vnn net 192.168.1.0/24
```

- 抓取网卡 eth0 上所有包含端口 22 的数据包
```shell
tcpdump -i eth0 -vnn port 22
```

- 抓取指定协议格式的数据包，协议格式可以是 [udp,icmp,arp,ip] 中的任何一种,例如以下命令：
```shell
tcpdump udp -i eth0 -vnn
```

- 抓取经过 eth0 网卡的源 ip 是 192.168.1.100 数据包，src参数表示源。
```shell
tcpdump -i eth0 -vnn src host 192.168.1.100
```

- 抓取经过 eth0 网卡目的 ip 是 192.168.1.100 数据包，dst参数表示目的。
```shell
tcpdump -i eth0 -vnn dst host 192.168.1.100
```

- 抓取源端口是 22 的数据包
```shell
tcpdump -i eth0 -vnn src port 22
```

- 抓取源 ip 是 192.168.1.100 且目的 ip 端口是 22 的数据包
```shell
tcpdump -i eth0 -vnn src host 192.168.1.100 and dst port 22
```

- 抓取源ip 192.168.1.100 22
```shell
tcpdump -i eth0 -vnn src host 192.168.1.100 or port 22
```

- 抓取源 ip 192.168.1.100 非 22
```shell
tcpdump -i eth0 -vnn src host 192.168.1.100 and not port 22
```

- 抓取源 ip 是 192.168.1.100 且目的端口是 22，或源 ip 是 192.168.1.102 且目的端口是 80 的数据包。
```shell
tcpdump -i eth0 -vnn ( src host 192.168.1.100 and dst port 22 ) or ( src host 192.168.1.102 and dst port 80 )
```

- 抓取100个数据包后就退出程序。
```shell
tcpdump –i eth0 -vnn -w /tmp/tcpdump -c 100
```
- 从 /tmp/tcpdump 记录中读取 tcp 协议的数据包
```shell
tcpdump -i eth0  tcp  -vnn -r /tmp/tcpdump
```

- 想要截获所有 192.168.1.100 的主机收到的和发出的所有的数据包：
```shell
tcpdump host 192.168.1.100
```

- 如果想要获取主机 192.168.1.100 除了和主机 192.168.1.101 之外所有主机通信的 ip 包，使用命令：
```shell
tcpdump ip host 192.168.1.100 and ! 192.168.1.101
```

- 如果想要获取主机 192.168.1.100 接收或发出的 telnet 包，使用如下命令：
```shell
tcpdump tcp port 23 host 192.168.1.100
```
