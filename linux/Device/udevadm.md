### 选项
info      查询sysfs或者udev的数据库
trigger 从内核请求events
settle   查看udev事件队列，如果所有的events已处理则退出
control  修改udev后台的内部状态信息
monitor 监控内核的uevents
hwdb    处理硬件数据库索引
test      调试
--debug   在标准错误(STDERR)上显示调试信息 
--version  显示版本信息 
-h, --help 显示帮助信息 
udevadm info [options] [devpath|file] 
从udev数据库中提取设备信息。 此外，还可以从sysfs中提取设备的属性， 以帮助创建与此设备匹配的udev规则。 

-q, --query=TYPE 

提取特定类型的设备信息。 必须和 --path 或 --name 选项连用。 TYPE 可以是下列值之一： name, symlink, path, property, all(默认值) 

-p, --path=DEVPATH 

该设备在 /sys 目录下的路径(例如 [/sys]/class/block/sda)。 因为 udev 能够猜测参数的意义， 所以通常将 --devpath=/class/block/sda 直接简写为 /sys/class/block/sda 

[devpath|file] 可用作 --path|--name 选项的简写替代，但是必须使用 /sys 或 /dev 开头的绝对路径。 

udevadm trigger [options] [devpath|file...]¶ 

强制内核触发设备事件，主要用于重放内核初始化过程中的冷插(coldplug)设备事件。 

-n, --name=FILE 

设备节点或软连接的名称(例如 [/dev]/sda)。 因为 udev 能够猜测参数的意义， 所以通常将 --name=sda 直接简写为 /dev/sda 

-r, --root 

以绝对路径显示 --query=name 与 --query=symlink 的查询结果 

-a, --attribute-walk 

按照udev规则的格式，显示所有可用于匹配该设备的sysfs属性： 从该设备自身开始，沿着设备树向上回溯(一直到树根)，显示沿途每个设备的sysfs属性。 

-x, --export 

以 key=value 的格式输出此设备的属性 

-P, --export-prefix=NAME 

在输出的键名前添加一个前缀。 

-d, --device-id-of-file=FILE 

显示 FILE 文件所在底层设备的主/次设备号。 

-e, --export-db 

导出udev数据库的全部内容 

-c, --cleanup-db 

清除udev数据库 

-v, --verbose 

显示被触发的设备列表 

-n, --dry-run 

并不真正触发设备事件 

-t, --type=TYPE 

仅触发特定类型的设备，TYPE 可以是下列值之一： devices(默认值), subsystems 

-c, --action=ACTION 

指定触发哪种类型的设备事件，ACTION 可以是下列值之一： add, remove, change(默认值) 

-s, --subsystem-match=SUBSYSTEM 

仅触发属于 SUBSYSTEM 子系统的设备事件。 可以多次使用此选项， 并且可以在 SUBSYSTEM 中使用shell风格的通配符。 

-S, --subsystem-nomatch=SUBSYSTEM 

不触发属于 SUBSYSTEM 子系统的设备事件。 可以多次使用此选项，并且可以在 SUBSYSTEM 中使用shell风格的通配符。 

-a, --attr-match=ATTRIBUTE=VALUE 

仅触发那些在设备的sysfs目录中存在 ATTRIBUTE 文件的设备事件。 如果同时还指定了"=VALUE"， 那么表示仅触发那些 ATTRIBUTE 文件的内容等于 VALUE 的设备事件。 可以多次使用此选项， 并且可以在 VALUE 中使用shell风格的通配符。 

-A, --attr-nomatch=ATTRIBUTE=VALUE 

不触发那些在设备的sysfs目录中存在 ATTRIBUTE 文件的设备事件。 如果同时还指定了"=VALUE"， 那么表示不触发那些 ATTRIBUTE 文件的内容等于 VALUE 的设备事件。 可以多次使用此选项， 并且可以在 VALUE 中使用shell风格的通配符。 

-p, --property-match=PROPERTY=VALUE 

仅触发那些设备的 PROPERTY 属性值等于 VALUE 的设备事件。 可以多次使用此选项， 并且可以在 VALUE 中使用shell风格的通配符。 

-g, --tag-match=PROPERTY 

仅触发带有 PROPERTY 标签的设备事件。 可以多次使用此选项。 

-y, --sysname-match=SYSNAME 

仅触发设备路径的末尾(也就是该设备在 /sys 路径下的文件名)为 SYSNAME 的设备事件。 可以多次使用此选项，并且可以在 SYSNAME 中使用shell风格的通配符。 

 --name-match=NAME 

触发给定设备及其所有子设备的事件。 NAME 是该设备在 /dev 目录下的路径。 

-b, --parent-match=SYSPATH 

触发给定设备及其所有子设备的事件。 SYSPATH 是该设备在 /sys 目录下的路径。 

[devpath|file...] 可用作 --parent-match|--name-match 选项的简写替代，但是必须使用 /sys 或 /dev 开头的绝对路径。 

  

udevadm settle [options] ¶ 

监视udev事件队列，并且在所有事件全部处理完成之后退出。 

-t, --timeout=SECONDS 

最多允许花多少秒等候事件队列清空。 默认值是120秒。 设为 0 表示仅检查事件队列是否为空， 并且立即返回。 

-E, --exit-if-exists=FILE 

如果 FILE 文件存在，则停止等待 

udevadm control command¶ 

控制udev守护进程(systemd-udevd)的内部状态。 

-e, --exit 

向 systemd-udevd 发送"退出"信号并等待其退出。 

-l, --log-priority=value 

设置 systemd-udevd.service(8) 的内部日志等级。 可以用数字或文本表示： remerg(0), alert(1), crit(2), err(3), warning(4), notice(5), info(6), debug(7) 

-s, --stop-exec-queue 

向 systemd-udevd 发送"禁止处理事件"信号， 这样所有新发生的事件都将进入等候队列。 

 

-S, --start-exec-queue¶ 

向 systemd-udevd 发送"开始处理事件"信号，也就是开始处理事件队列中尚未处理的事件。 

  

-R, --reload¶ 

向 systemd-udevd 发送"重新加载"信号，也就是重新加载udev规则与各种数据库(包括内核模块索引)。 注意，重新加载之后并不影响已经存在的设备， 但是新的配置将会应用于所有将来发生的新设备事件。 

  

-p, --property=KEY=value¶ 

为所有将来发生的新设备事件统一设置一个全局的 KEY 属性，并将其值设为 value 

  

-m, --children-max=value¶ 

设置最多允许 systemd-udevd 同时处理多少个设备事件。 

  

--timeout=seconds¶ 

设置等候 systemd-udevd 应答的最大秒数 

  

-h, --help¶ 

显示帮助信息 

  

udevadm monitor [options] ¶ 

监视内核发出的设备事件(以"KERNEL"标记)， 以及udev在处理完udev规则之后发出的事件(以"UDEV"标记)，并在控制台上输出事件的设备路径(devpath)。 可用于分析udev处理设备事件所花的时间(比较"KERNEL"与"UDEV"的时间戳)。 

  

-k, --kernel¶ 

仅显示"KERNEL"事件 

  

-u, --udev¶ 

仅显示"UDEV"事件 

  

-p, --property¶ 

同时还显示事件的各属性 

  

-s, --subsystem-match=subsystem[/devtype]¶ 

根据 subsystem[/devtype] 对事件(包括 kernel uevent 与 udev event)进行过滤：仅显示与"子系统[/设备类型]"匹配的"UDEV"事件。 

  

-t, --tag-match=string¶ 

根据设备标签对事件(仅 udev event)进行过滤：仅显示与"标签"匹配的"UDEV"事件。 

  

-h, --help¶ 

显示帮助信息 

  

udevadm test [options] [devpath] ¶ 

模拟一个设备事件，并输出调试信息。 

  

-a, --action=ACTION¶ 

指定模拟哪种类型的设备事件，ACTION 可以是下列值之一：add(默认值), remove, change 

  

-N, --resolve-names=early|late|never¶ 

指定 udevadm 何时解析用户与组的名称： early(默认值) 表示在规则的解析阶段； late 表示在每个事件发生的时候； never 表示从不解析， 所有设备的属主与属组都是 root 。 

  

-h, --help¶ 

显示帮助信息 
udevadm test-builtin [options] [command] [devpath] ¶ 

针对 DEVPATH设备 运行一个内置的 COMMAND 命令， 并输出调试信息。 

-h, --help¶  显示帮助信息 
 
