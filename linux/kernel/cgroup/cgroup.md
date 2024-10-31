# cgroup

pid、io、memory、cpu

```shell

stat -fc %T /sys/fs/cgroup/
<!--对于 cgroup v2，输出为 cgroup2fs。-->
<!--对于 cgroup v1，输出为 tmpfs。-->
```


虽然cgroup v2早已在linux 4.5版本的时候就已经加入内核中了，而centos 8默认也已经用了4.18作为其内核版本，但是系统中仍然默认使用的是cgroup v1。

本文主要介绍了在fedora 31系统，内核版本为5.5.15上的cgroup v2使用方法。也是继前几年写的四篇cgroup文章后再次讲解cgroup。谁让我之前在那些文章里挖了坑呢？好吧，这篇是我欠你们的。祝阅读愉快。

## 在系统上开启cgroup v2

因为系统上默认仍然开启cgroup v1，所以我们需要配置一下系统，并且换成cgroup v2。为了确认切换是否成功，我们需要先看一下v1什么样？

```shell
[root@localhost]# mount
......
cgroup on /sys/fs/cgroup/rdma type cgroup (rw,nosuid,nodev,noexec,relatime,rdma)
cgroup on /sys/fs/cgroup/cpuset type cgroup (rw,nosuid,nodev,noexec,relatime,cpuset)
cgroup on /sys/fs/cgroup/perf_event type cgroup (rw,nosuid,nodev,noexec,relatime,perf_event)
cgroup on /sys/fs/cgroup/memory type cgroup (rw,nosuid,nodev,noexec,relatime,memory)
cgroup on /sys/fs/cgroup/net_cls,net_prio type cgroup (rw,nosuid,nodev,noexec,relatime,net_cls,net_prio)
cgroup on /sys/fs/cgroup/cpu,cpuacct type cgroup (rw,nosuid,nodev,noexec,relatime,cpu,cpuacct)
cgroup on /sys/fs/cgroup/freezer type cgroup (rw,nosuid,nodev,noexec,relatime,freezer)
cgroup on /sys/fs/cgroup/pids type cgroup (rw,nosuid,nodev,noexec,relatime,pids)
cgroup on /sys/fs/cgroup/hugetlb type cgroup (rw,nosuid,nodev,noexec,relatime,hugetlb)
cgroup on /sys/fs/cgroup/blkio type cgroup (rw,nosuid,nodev,noexec,relatime,blkio)
cgroup on /sys/fs/cgroup/devices type cgroup (rw,nosuid,nodev,noexec,relatime,devices)
......
```

mount命令中显示的这些cgroup的目录，就是v1的样子。下面我们切换一下v2，看看有什么区别。切换方法其实也很简单，就是在重新启动的时候加上一个内核引导参数：

systemd.unified_cgroup_hierarchy=1

这个参数的意思是，打开 cgroup 的 unified 属性。是的，unified 的 cgroup 就是v2了。我们加上参数重新引导之后看一下状态：

```shell
grubby --update-kernel=ALL --args=systemd.unified_cgroup_hierarchy=1
```

在 grub2 中加入相关内核参数，之后重启系统。

```shell
[root@localhost]# mount|grep cgroup
cgroup2 on /sys/fs/cgroup type cgroup2 (rw,nosuid,nodev,noexec,relatime,nsdelegate)
```

cgroup v1比v2啰嗦了不少，切换 v2 后世界清爽了很多。

我们再来看一下cgroup v2的目录树结构：

```shell
[root@localhost]# ls -p /sys/fs/cgroup/
cgroup.controllers      cgroup.stat             cpuset.cpus.effective  machine.slice/
cgroup.max.depth        cgroup.subtree_control  cpuset.mems.effective  memory.pressure
cgroup.max.descendants  cgroup.threads          init.scope/            system.slice/
cgroup.procs            cpu.pressure            io.pressure            user.slice/
```

从目录树结构上看，cgroup v2相比v1变化还是很大的。我们基本要重新学习一下如何配置cgroup v2。

## 新建一个cgroup

根 v1 类似，我们也是通过在 cgroup 相关目录下创建新的目录来创建 cgoup 控制对象的。比如我们想创建一个叫 zorro 的 cgroup 组：

```auto
[root@localhost]# cd /sys/fs/cgroup/
[root@localhost cgroup]# mkdir zorro
[root@localhost cgroup]# ls zorro/
cgroup.controllers      cgroup.stat             io.pressure     memory.min           memory.swap.max
cgroup.events           cgroup.subtree_control  memory.current  memory.oom.group     pids.current
cgroup.freeze           cgroup.threads          memory.events   memory.pressure      pids.events
cgroup.max.depth        cgroup.type             memory.high     memory.stat          pids.max
cgroup.max.descendants  cpu.pressure            memory.low      memory.swap.current
cgroup.procs            cpu.stat                memory.max      memory.swap.events
```


- cgroup.controllers:
这个文件显示了当前 cgoup 可以限制的相关资源有哪些？
v2 之所以叫 unified，除了在内核中实现架构的区别外，体现在外在配制方法上也有变化。
比如，这一个文件就可以控制当前 cgroup 都支持哪些资源的限制。
而不是像v1一样资源分别在不同的目录下进行创建相关 cgroup。

默认创建出来的 zorro 组中的 cgroup.controllers 内容为：

```shell
[root@localhost cgroup]# cat zorro/cgroup.controllers
memory pids
```

表示当前 cgroup 只支持针对 memory 和 pids 的限制。
如果我们要创建可以支持更多资源限制能力的组，就要去其上一级目录的文件中查看，整个 cgroup 可以支持的资源限制有哪些？

```shell
[root@localhost zorro]# cat /sys/fs/cgroup/cgroup.controllers
cpuset cpu io memory pids
```

当前 cgroup 可以支持 cpuset cpu io memory pids 的资源限制。

- cgroup.subtree_control: 
这个文件内容应是 cgroup.controllers 的子集。
其作用是限制在当前 cgroup 目录层级下创建的子目录中的 cgroup.controllers 内容。
就是说，子层级的 cgroup 资源限制范围被上一级的 cgroup.subtree_control 文件内容所限制。

所以，如果我们想创建一个可以支持 cpuset cpu io memory pids 全部五种资源限制能力的 cgroup 组的话，应该做如下操作：

```shell
[root@localhost]# cat /sys/fs/cgroup/cgroup.controllers
cpuset cpu io memory pids
[root@localhost]# cat /sys/fs/cgroup/cgroup.subtree_control
cpu memory pids
[root@localhost]# echo '+cpuset +cpu +io +memory +pids' > /sys/fs/cgroup/cgroup.subtree_control
[root@localhost]# cat !$
cat /sys/fs/cgroup/cgroup.subtree_control
cpuset cpu io memory pids
[root@localhost]# mkdir /sys/fs/cgroup/zorro
[root@localhost]# cat /sys/fs/cgroup/zorro/cgroup.controllers
cpuset cpu io memory pids
[root@localhost]# cat /sys/fs/cgroup/zorro/cgroup.subtree_control
[root@localhost]# ls /sys/fs/cgroup/zorro/
cgroup.controllers      cpu.pressure           io.max               memory.oom.group
cgroup.events           cpu.stat               io.pressure          memory.pressure
cgroup.freeze           cpu.weight             io.stat              memory.stat
cgroup.max.depth        cpu.weight.nice        io.weight            memory.swap.current
cgroup.max.descendants  cpuset.cpus            memory.current       memory.swap.events
cgroup.procs            cpuset.cpus.effective  memory.events        memory.swap.max
cgroup.stat             cpuset.cpus.partition  memory.events.local  pids.current
cgroup.subtree_control  cpuset.mems            memory.high          pids.events
cgroup.threads          cpuset.mems.effective  memory.low           pids.max
cgroup.type             io.bfq.weight          memory.max
cpu.max                 io.latency             memory.min
```

此时我们创建的 zorro 组就有 cpu，cpuset，io，memory，pids 等常见的资源限制能力了。
另外要注意，被限制进程只能添加到叶子结点的组中，不能添加到中间结点的组内。

我们再来看一下其他cgroup开头的文件说明：
- cgroup.events: 
    包含两个只读的 key-value。
    populated:
        1 表示当前 cgroup 内有进程，
        0 表示没有。
    frozen:
        1 表示当前 cgroup 为 frozen 状态，
        0 表示非此状态。
- cgroup.type:
    表示当前cgroup的类型，cgroup 类型包括: "domain"：默认类型。
    "domain threaded": 作为threaded类型cgroup的跟结点。
    "domain invalid": 无效状态cgroup。
    "threaded": threaded类型的cgoup组。

这里引申出一个新的知识，
即：cgroup v2支持threaded模式。
所谓threaded模式其本质就是控制对象从进程为单位支持到了线程为单位。
我们可以在一个由domain threaded类型的组中创建多个threaded类型的组，并把一个进程的多个线程放到不同的threaded类型组中进行资源限制。
创建threaded类型cgroup的方法就是把cgroup.type改为对应的类型即可。

- cgroup.procs: 
    查看这个文件显示的是当前在这个 cgroup 中的 pid list。
    echo 一个 pid 到这个文件可以将对应进程放入这个组中进行资源限制。
- cgroup.threads: 
    跟上一个文件概念相同，区别是针对tid进行控制。
- cgroup.max.descendants: 
    当前cgroup目录中可以允许的最大子cgroup个数。默认值为max。
- cgroup.max.depth: 
    当前cgroup目录中可以允许的最大cgroup层级数。默认值为max。
- cgroup.stat: 
    包含两个只读的key-value。
    nr_descendants: 当前cgroup下可见的子孙cgroup个数。
    nr_dying_descendants: 这个cgroup下曾被创建但是已被删除的子孙cgroup个数。
- cgroup.freeze：值为1可以将cgroup值为freeze状态。默认值为0，

当然，相关说明大家也可以在内核源代码中的：Documentation/admin-guide/cgroup-v2.rst 找到其解释。



## 最后

以上是cgroup v2的配置说明。我们会发现，跟v1相比，新版cgroup配置上的复杂度要小很多。
并且加入了包括buffer io限制和psi等新的功能。新版cgroup也放弃了配置网络资源隔离的接口，
当然需要的话，网络资源隔离部分还是可以直接使用tc进行配置。
