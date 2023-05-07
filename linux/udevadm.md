# udevadm
udev 管理工具

### 选项
--debug   在标准错误(STDERR)上显示调试信息 
--version  显示版本信息 
-h, --help 显示帮助信息 
---
udevadm info [options] [devpath|file] 
info      查询sysfs或者udev的数据库
    -q, --query=TYPE ：从数据库中查询指定类型的设备，需要使用 --path 和 --name 选项指定设备。合法的 TYPE 类型包括：设备名（name），链接（symlink），路径（path），属性（property）。
    -p, --path=DEVPATH ：指定设备的路径。
    -n, --name=FILE ：指定设备节点或者链接。
    -a, --attribute-walk ：打印指定设备的所有 sysfs 记录的属性，以用 udev 规则来匹配特殊的设备。该选项打印链上的所有设备信息，最大可能到 sys 目录。
    -d, --device-id-of-file=FILE ：打印主/从设备号。
    -e, --export-db ：输出 udev 数据库中的内容。
---
trigger 接收内核发送来的设备事件，主要用于重放 coldplug 事件信息
    -v, --verbose ：输出将要被触发的设备列表。
    -n, --dry-run ：不真的触发事件。
    -t, --type=TYPE ：触发一个特殊的设备，合法的类型有 devices 和 subsystem，默认为 devices。
    -c, --action=ACTION ：被触发的事件，默认是 change。
    -s, --subsystem-match=SUBSYSTEM ：触发匹配子系统的设备事件，这个选项可以被多次指定，并且支持 shell 模式匹配。
    -a, --attr-match=ATTRIBUTE=VALUE ：触发匹配 sysfs 属性的设备事件，如果属性值和属性一起指定，属性的值可以使用 shell 模式匹配；如果没有指定值，会重新确认现有属性。（这个选项可以被多次指定）
    -A, --attr-nomatch=ATTRIBUTE=VALUE ：不要触发匹配属性的设备事件，如果可以使用模式匹配，也可以多次指定。
    -p, --property-match=PROPERTY=VALUE ：匹配与属性吻合的设备，可以多次指定支持模式匹配。
    -g, --tag-match=PROPERTY ：匹配与标签吻合的设备，可以多次指定。
    -y, --sysname-match=NAME ：匹配与 sys 设备名相同的设备，可以多次指定支持模式匹配。
---
settle   查看udev事件队列，如果所有的events已处理则退出
    -t, --timeout=SECONDS ：等待事件队列变空的最大等待时间，默认是120 秒，如果为 0 则立即退出。
    -E, --exit-if-exists=FILE ：如果文件存在就退出。
---
monitor 监听内核事件和 udev 发送的 events 事件，可以通过比较内核或者 udev 事件的时间戳来分析事件时序
    -k, --kernel ：输出内核事件。
    -u, --udev ：输出 udev 规则执行时的 udev 事件。
    -p, --property ：输出事件的属性。
    -s, --subsystem-match=string[/string] ：通过子系统或者设备类型过滤事件，只有匹配了子系统值的 udev 设备事件通过。
    -t, --tag-match=string ：通过属性过滤事件，只有匹配了标签的 udev 事件通过。
---
control  修改udev后台的内部状态信息
---
hwdb 处理硬件数据库索引
---
test 模拟一个 udev 事件，打印出 debug 信息
---

## 例子
- 查询块设备 /dev/sdc1 的路径信息
```shell
udevadm info -q path -n /dev/sdc1
/devices/pci0000:00/0000:00:14.0/usb2/2-1/2-1:1.0/host4/target4:0:0/4:0:0:0/block/sdc/sdc1
```

- 查询 RTC 设备的所有信息
```shell
udevadm info --query=all --name=/dev/rtc
P: /devices/platform/i2c@4/i2c-4/4-006f/rtc/rtc0
N: rtc0
L: -100
S: rtc
E: DEVPATH=/devices/platform/i2c@4/i2c-4/4-006f/rtc/rtc0
E: DEVNAME=/dev/rtc0
E: MAJOR=252
E: MINOR=0
E: SUBSYSTEM=rtc
E: USEC_INITIALIZED=5168933
E: DEVLINKS=/dev/rtc
```

- 不重启设备直接 reload udev 规则
```shell
udevadm control --reload-rules && udevadm trigger
```
