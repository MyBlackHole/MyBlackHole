# receive

接收快照流
接收端 bookmark 没有

## 选项

zfs receive [-vnFu] filesystem|vol|snapshot
zfs receive [-vnFu] [-d|-e] filesystem
 
创建一个快照，其内容与标准输入上提供的流中指定的相同。 如果接收到完整的流，则也会创建一个新的文件系统。 使用 zfs 创建流
send 子命令，默认情况下会创建完整的流。 zfs recv 可以用作 zfs receive 的别名。

如果接收到增量流，则目标文件系统必须已经存在，并且其最新快照必须与增量流的源匹配。 对于 zvols，目标设备链接
被销毁并重新创建，这意味着在接收操作期间无法访问 zvol。

收到使用 zfs send -R 命令生成的快照复制包流时，将使用 zfs destroy 销毁发送位置上不存在的任何快照

-d 命令。

该子命令创建的快照（以及文件系统，如果收到完整流）的名称取决于参数类型以及 -d 或 -e 选项的使用。

如果参数是快照名称，则创建指定的快照。 如果参数是文件系统或卷名称，则在指定的时间内创建与发送的快照同名的快照
文件系统或卷。 如果未指定 -d 或 -e 选项，则将完全按照提供的方式使用所提供的目标快照名称。

-d 和 -e 选项导致通过将发送的快照名称的一部分附加到指定的目标文件系统来确定目标快照的文件系统名称。 如果 -d 选项是指定的
已发送的快照的文件系统路径（通常是池名称）中除了第一个元素之外的所有元素都将被使用，并且指定的文件系统中任何所需的中间文件系统都会被创建。 如果-e选项
指定，则仅将发送快照的文件系统名称的最后一个元素（即源文件系统本身的名称）用作目标文件系统名称。
 
### d
丢弃已发送快照的文件系统名称的第一个元素，使用其余元素来确定新快照的目标文件系统的名称，如本段中所述多于。
 
### e
丢弃已发送快照的文件系统名称中除最后一个元素以外的所有元素，使用该元素来确定新快照的目标文件系统的名称，如上段所述。

### u
未安装与接收到的流关联的文件系统。

### v
打印有关流的详细信息以及执行接收操作所需的时间。

### n
实际上并不接收流。 这可以与 -v 选项结合使用来验证接收操作将使用的名称。

### F
在执行接收操作之前强制将文件系统回滚到最新快照。 如果接收增量复制流（例如，由 zfs send -R 生成的流）
-[iI])，销毁发送端不存在的快照和文件系统。

```shell
[root@node2 ~]# zfs send aiopool/4089108a1417_20_41_1690442448_goldendb_data@1690442862614 | zfs recv aiopool/bookmark
```

```shell
ssh root@xxxxxxxxxxxxxx -p 22 zfs send -I aiopool/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx@1690788378 aiopool/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx1690863820 | zfs receive -F aiopool/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```
