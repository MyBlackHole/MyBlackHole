# udev

如果你使用Linux比较长时间了，那你就知道，在对待设备文件这块，Linux改变了几次策略。在Linux早期，设备文件仅仅是是一些带有适当的属性集的普通文件，它由mknod命令创建，文件存放在/dev目录下。后来，采用了devfs,一个基于内核的动态设备文件系统，他首次出现在2.3.46 内核中。Mandrake，Gentoo等Linux分发版本采用了这种方式。devfs创建的设备文件是动态的。但是devfs有一些严重的限制，从 2.6.13版本后移走了。目前取代他的便是文本要提到的udev－－一个用户空间程序。

目前很多的Linux分发版本采纳了udev的方式，因为它在Linux设备访问，特别是那些对设备有极端需求的站点（比如需要控制上千个硬盘）和热插拔设备（比如USB摄像头和MP3播放器）上解决了几个问题。下面我我们来看看如何管理udev设备。

实际上，对于那些为磁盘，终端设备等准备的标准配置文件而言，你不需要修改什么。但是，你需要了解udev配置来使用新的或者外来设备，如果不修改配置，这些设备可能无法访问，或者说Linux可能会采用不恰当的名字，属组或权限来创建这些设备文件。你可能也想知道如何修改RS－232串口，音频设备等文件的属组或者权限。这点在实际的Linux实施中是会遇到的。

## 为什么使用udev

在此之前的设备文件管理方法（静态文件和devfs）有几个缺点：

- 不确定的设备映射。特别是那些动态设备，比如USB设备，设备文件到实际设备的映射并不可靠和确定。举一个例子：如果你有两个USB打印机。一个可能称为/dev/usb/lp0,另外一个便是/dev/usb/lp1。但是到底哪个是哪个并不清楚，lp0,lp1和实际的设备没有一一对应的关系，因为他可能因为发现设备的顺序，打印机本身关闭等原因而导致这种映射并不确定。理想的方式应该是：两个打印机应该采用基于他们的序列号或者其他标识信息的唯一设备文件来映射。但是静态文件和devfs都无法做到这点。

*没有足够的主/辅设备号。我们知道，每一个设备文件是有两个8位的数字：一个是主设备号 ，另外一个是辅设备号来分配的。这两个8位的数字加上设备类型（块设备或者字符设备）来唯一标识一个设备。不幸的是，关联这些身边的的数字并不足够。

*/dev目录下文件太多。一个系统采用静态设备文件关联的方式，那么这个目录下的文件必然是足够多。而同时你又不知道在你的系统上到底有那些设备文件是激活的。

*命名不够灵活。尽管devfs解决了以前的一些问题，但是它自身又带来了一些问题。其中一个就是命名不够灵活；你别想非常简单的就能修改设备文件的名字。缺省的devfs命令机制本身也很奇怪，他需要修改大量的配置文件和程序。;

*内核内存使用，devfs特有的另外一个问题是，作为内核驱动模块，devfs需要消耗大量的内存，特别当系统上有大量的设备时（比如上面我们提到的系统一个上有好几千磁盘时）

udev的目标是想解决上面提到的这些问题，他通采用用户空间(user-space)工具来管理/dev/目录树，他和文件系统分开。知道如何改变缺省配置能让你之大如何定制自己的系统，比如创建设备字符连接，改变设备文件属组，权限等。

## udev配置文件

主要的udev配置文件是/etc/udev/udev.conf。这个文件通常很短，他可能只是包含几行#开头的注释，然后有几行选项：  
 udev_root="/dev/"

udev_rules="/etc/udev/rules.d/"

udev_log="err"  
 上面的第二行非常重要，因为他表示udev规则存储的目录，这个目录存储的是以.rules结束的文件。每一个文件处理一系列规则来帮助udev分配名字给设备文件以保证能被内核识别。  
 你的/etc/udev/rules.d下面可能有好几个udev规则文件，这些文件一部分是udev包安装的，另外一部分则是可能是别的硬件或者软件包生成的。比如在Fedora Core 5系统上，sane-backends包就会安装60-libsane.rules文件，另外initscripts包会安装60-net.rules文件。这些规则文件的文件名通常是两个数字开头，它表示系统应用该规则的顺序。

规则文件里的规则有一系列的键/值对组成，键/值对之间用逗号(,)分割。每一个键或者是用户匹配键，或者是一个赋值键。匹配键确定规则是否被应用，而赋值键表示分配某值给该键。这些值将影响udev创建的设备文件。赋值键可以处理一个多值列表。

## 规则说明[#](https://www.cnblogs.com/my-show-time/p/15108401.html#%E8%A7%84%E5%88%99%E8%AF%B4%E6%98%8E)

### 1、udev 规则的所有操作符[#](https://www.cnblogs.com/my-show-time/p/15108401.html#1udev-%E8%A7%84%E5%88%99%E7%9A%84%E6%89%80%E6%9C%89%E6%93%8D%E4%BD%9C%E7%AC%A6)

“==”：　　比较键、值，若等于，则该条件满足；  
 “!=”： 　　比较键、值，若不等于，则该条件满足；  
 “=”： 　　 对一个键赋值；  
 “+=”：　　为一个表示多个条目的键赋值。  
 “:=”：　　对一个键赋值，并拒绝之后所有对该键的改动。目的是防止后面的规则文件对该键赋值。

### 2、udev 规则的匹配键[#](https://www.cnblogs.com/my-show-time/p/15108401.html#2udev-%E8%A7%84%E5%88%99%E7%9A%84%E5%8C%B9%E9%85%8D%E9%94%AE)

ACTION： 　　 　　　　　　事件 (uevent) 的行为，例如：add( 添加设备 )、remove( 删除设备 )。  
 KERNEL： 　　 　　　　　　内核设备名称，例如：sda, cdrom。  
 DEVPATH：　　　　　　　 设备的 devpath 路径。  
 SUBSYSTEM： 　　　　　　 设备的子系统名称，例如：sda 的子系统为 block。  
 BUS： 　　　　　　　　　　 设备在 devpath 里的总线名称，例如：usb。  
 DRIVER： 　　　　 　　　　 设备在 devpath 里的设备驱动名称，例如：ide-cdrom。  
 ID： 　　　　　　　 　　　　设备在 devpath 里的识别号。  
 SYSFS{filename}： 　　　　设备的 devpath 路径下，设备的属性文件“filename”里的内容。  
 　　　　　　　　　　　　　　　例如：SYSFS{model}==“ST936701SS”表示：如果设备的型号为 ST936701SS，则该设备匹配该 匹配键。  
 　　　　　　　　　　　　　　　在一条规则中，可以设定最多五条 SYSFS 的 匹配键。  
 ENV{key}： 　　　　　　 　 环境变量。在一条规则中，可以设定最多五条环境变量的 匹配键。  
 PROGRAM：　　　　　　　　调用外部命令。  
 RESULT： 　　　　　　　　 外部命令 PROGRAM 的返回结果。

### 3、udev 的重要赋值键[#](https://www.cnblogs.com/my-show-time/p/15108401.html#3udev-%E7%9A%84%E9%87%8D%E8%A6%81%E8%B5%8B%E5%80%BC%E9%94%AE)

NAME：　　　　　　　　　　　在 /dev下产生的设备文件名。只有第一次对某个设备的NAME 的赋值行为生效，之后匹配的规则再对该设备的 NAME 赋值行为将被忽略。如果没有任何规则对设备的 NAME 赋值，udev 将使用内核设备名称来产生设备文件。  
 SYMLINK：　　　　　　　　　 为 /dev/下的设备文件产生符号链接。由于 udev 只能为某个设备产生一个设备文件，所以为了不覆盖系统默认的 udev 规则所产生的文件，推荐使用符号链接。  
 OWNER, GROUP, MODE：　　为设备设定权限。  
 ENV{key}：　　　　　　　　　导入一个环境变量。

### 4、udev 的值和可调用的替换操作符[#](https://www.cnblogs.com/my-show-time/p/15108401.html#4udev-%E7%9A%84%E5%80%BC%E5%92%8C%E5%8F%AF%E8%B0%83%E7%94%A8%E7%9A%84%E6%9B%BF%E6%8D%A2%E6%93%8D%E4%BD%9C%E7%AC%A6)

Linux 用户可以随意地定制 udev 规则文件的值。例如：my_root_disk, my_printer。同时也可以引用下面的替换操作符：  
 $kernel, %k：　　　　　　　　设备的内核设备名称，例如：sda、cdrom。  
 $number, %n：　　　　　　　 设备的内核号码，例如：sda3 的内核号码是 3。  
 $devpath, %p：　　　　　　　设备的 devpath路径。  
 $id, %b：　　　　　　　　　　设备在 devpath里的 ID 号。  
 $sysfs{file}, %s{file}：　　 设备的 sysfs里 file 的内容。其实就是设备的属性值。  
 $env{key}, %E{key}：　　　一个环境变量的值。  
 $major, %M：　　　　　　　　设备的 major 号。  
 $minor %m：　　　　　　　　设备的 minor 号。  
 $result, %c：　　　　　　　　PROGRAM 返回的结果。  
 $parent, %P：　　　　　　 父设备的设备文件名。  
 $root, %r：　　　　　　　　 udev_root的值，默认是 /dev/。  
 $tempnode, %N：　　　　　　临时设备名。  
 %%：　　　　　　　　　　　　符号 % 本身。  
 $$：　　　　　　　　　　　　　符号 $ 本身。

### EXAMPLES[#](https://www.cnblogs.com/my-show-time/p/15108401.html#examples)

给出一个列子来解释如何使用这些键， 下面的例子来自Fedora Core 5系统的标准配置文件：  
 KERNEL"*", OWNER="root" GROUP="root", MODE="0600"  
 KERNEL"tty", NAME="%k", GROUP="tty", MODE="0666", OPTIONS="last_rule"  
 KERNEL"scd[0-9]*", SYMLINK+="cdrom cdrom-%k"  
 KERNEL"hd[a-z]", BUS"ide", SYSFS{removable}"1", SYSFS{device/media}"cdrom", SYMLINK+="cdrom cdrom-%k"  
 ACTION"add", SUBSYSTEM=="scsi_device", RUN+="/sbin/modprobe sg"  
 上面的例子给出了5个规则，每一个都是KERNEL或者ACTION键开头：

*第一个规则是缺省的，他匹配任意被内核识别到的设备，然后设定这些设备的属组是root，组是root，访问权限模式是0600(-rw-------)。这也是一个安全的缺省设置保证所有的设备在默认情况下只有root可以读写。

*第二个规则也是比较典型的规则了。它匹配终端设备(tty)，然后设置新的权限为0600，所在的组是tty。它也设置了一个特别的设备文件名:%K。在这里例子里，%k代表设备的内核名字。那也就意味着内核识别出这些设备是什么名字，就创建什么样的设备文件名。

第三行开始的KERNEL=="scd[0-9]",表示 SCSI CD-ROM 驱动. 它创建一对设备符号连接：cdrom和cdrom-%k。

*第四行，开始的 KERNEL=="hd[a-z]", 表示ATA CDROM驱动器。这个规则创建和上面的规则相同的符号连接。ATA CDROM驱动器需要sysfs值以来区别别的ATA设备，因为SCSI CDROM可以被内核唯一识别。.

*第五行以 ACTION=="add"开始，它告诉udev增加 /sbin/modprobe sg 到命令列表，当任意SCSI设备增加到系统后，这些命令将执行。其效果就是计算机应该会增加sg内核模块来侦测新的SCSI设备。

当然，上面仅仅是一小部分例子，如果你的系统采用了udev方式，那你应该可以看到更多的规则。如果你想修改设备的权限或者创建信的符号连接，那么你需要熟读这些规则，特别是要仔细注意你修改的那些与之相关的设备。

## 修改你的udev配置[#](https://www.cnblogs.com/my-show-time/p/15108401.html#%E4%BF%AE%E6%94%B9%E4%BD%A0%E7%9A%84udev%E9%85%8D%E7%BD%AE)

在修改udev配置之前，我们一定要仔细，通常的考虑是：你最好不要修改系统预置的那些规则，特别不要指定影响非常广泛的配置，比如上面例子中的第一行。不正确的配置可能会导致严重的系统问题或者系统根本就无法这个正确的访问设备。

而我们正确的做法应该是在/etc/udev/rules.d/下创建一个信的规则文件。确定你给出的文件的后缀是rules文件名给出的数字序列应该比标准配置文件高。比如，你可以创建一个名为99-my-udev.rules的规则文件。在你的规则文件中，你可以指定任何你想修改的配置，比如，假设你修改修改floppy设备的所在组，还准备创建一个信的符号连接/dev/floppy，那你可以这么写：  
 KERNEL=="fd[0-9]*", GROUP="users", SYMLINK+="floppy"

有些发行版本，比如Fedora，采用了外部脚本来修改某些特定设备的属组，组关系和权限。因此上面的改动可能并不见得生效。如果你遇到了这个问题，你就需要跟踪和修改这个脚本来达到你的目的。或者你可以修改PROGRAM或RUN键的值来做到这点。

某些规则的修改可能需要更深的挖掘。比如，你可能想在一个设备上使用sysfs信息来唯一标识一个设备。这些信息最好通过udevinfo命令来获取。

$ udevinfo –a –p $(udevinfo –q path –n /dev/hda)  
 

上面的命令两次使用udevinfo：一次是返回sysfs设备路径(他通常和我们看到的Linux设备文件名所在路径－－/dev/hda－－不同)；第二次才是查询这个设备路径，结果将是非常常的syfs信息汇总。你可以找到最够的信息来唯一标志你的设备，你可以采用适当的替换udev配置文件中的 SYSFS选项。下面的结果就是上面的命令输出

[root@localhost rules.d]#  udevinfo -a -p $(udevinfo -q path -n /dev/hda1)  
  
Udevinfo starts with the device specified by the devpath and then  
walks up the chain of parent devices. It prints for every device  
found, all possible attributes in the udev rules key format.  
A rule to match, can be composed by the attributes of the device  
and the attributes from one single parent device.  
  
looking at device '/block/hda/hda1':  
KERNEL=="hda1"  
SUBSYSTEM=="block"  
DRIVER==""  
ATTR{stat}==" 1133 2268 2 4"  
ATTR{size}=="208782"  
ATTR{start}=="63"  
ATTR{dev}=="3:1"  
  
looking at parent device '/block/hda':  
KERNELS=="hda"  
SUBSYSTEMS=="block"  
DRIVERS==""  
ATTRS{stat}==" 28905 18814 1234781 302540 34087 133247 849708 981336 0 218340 1283968"  
ATTRS{size}=="117210240"  
ATTRS{removable}=="0"  
ATTRS{range}=="64"  
ATTRS{dev}=="3:0"  
  
looking at parent device '/devices/pci0000:00/0000:00:1f.1/ide0/0.0':  
KERNELS=="0.0"  
SUBSYSTEMS=="ide"  
DRIVERS=="ide-disk"  
ATTRS{modalias}=="ide:m-disk"  
ATTRS{drivename}=="hda"  
ATTRS{media}=="disk"  
  
looking at parent device '/devices/pci0000:00/0000:00:1f.1/ide0':  
KERNELS=="ide0"  
SUBSYSTEMS==""  
DRIVERS==""  
  
looking at parent device '/devices/pci0000:00/0000:00:1f.1':  
KERNELS=="0000:00:1f.1"  
SUBSYSTEMS=="pci"  
DRIVERS=="PIIX_IDE"  
ATTRS{broken_parity_status}=="0"  
ATTRS{enable}=="1"  
ATTRS{modalias}=="pci:v00008086d000024CAsv0000144Dsd0000C009bc01sc01i8a"  
ATTRS{local_cpus}=="1"  
ATTRS{irq}=="11"  
ATTRS{class}=="0x01018a"  
ATTRS{subsystem_device}=="0xc009"  
ATTRS{subsystem_vendor}=="0x144d"  
ATTRS{device}=="0x24ca"  
ATTRS{vendor}=="0x8086"  
  
looking at parent device '/devices/pci0000:00':  
KERNELS=="pci0000:00"  
SUBSYSTEMS==""  
DRIVERS==""  
 

### 例子一：修改USB扫描仪的配置。[#](https://www.cnblogs.com/my-show-time/p/15108401.html#%E4%BE%8B%E5%AD%90%E4%B8%80%E4%BF%AE%E6%94%B9usb%E6%89%AB%E6%8F%8F%E4%BB%AA%E7%9A%84%E9%85%8D%E7%BD%AE)

通过一系列的尝试，你已经为这个扫描仪标识了Linux设备文件(每次打开扫描仪时，名字都会变)。

你可以使用上面的命令替换这个正确的Linux设备文件名，然后定位输出的采用SYSFS{idVendor}行和SYSFS{idProduct}行。

最后你可以使用这些信息来为这个扫描仪创建新的选项。

SYSFS{idVendor}=="0686", \  
  
SYSFS{idProduct}=="400e", \  
  
SYMLINK+="scanner", MODE="0664", \  
  
group="scanner"  
 

上面的例子表示将扫描仪的组设置为scanner，访问权限设置为0664,同时创建一个/dev/scanner的符号连接。

### 例子二：使用udev修改u盘设备文件名[#](https://www.cnblogs.com/my-show-time/p/15108401.html#%E4%BE%8B%E5%AD%90%E4%BA%8C%E4%BD%BF%E7%94%A8udev%E4%BF%AE%E6%94%B9u%E7%9B%98%E8%AE%BE%E5%A4%87%E6%96%87%E4%BB%B6%E5%90%8D)

编写我们的/etc/udev/rules.d/10-local.rules文件

sudo vim /etc/udev/rules.d/10-local.rules

在里面加入这几个变量信息，如下：

KERNEL"sdc4",SUBSYSTEMS"block", NAME+="kinstonusb",SYMLINK+="kinstonusb_link"  
 上面的KERNEL"sdc4",SUBSYSTEMS"block"我们可以根据上面的输出直接拷贝过去的。我们保存这个文件。

一般我们要使这个规则文件生效，要热插拔我们的设备以产生一个事件或在设备中的event文件中增加信息以达到发送事件的目的来更新我们的udev规则，但这里有个更加方便的方法，我们可以运行下面这个命令。

sudo udevadm test /sys/class/block/sdc4

这样我们就更新了我们的规则。

看下/dev里的情况，如下图：

看下sdc，如下图：

sdc4不见了，即是我们命名我们的sdc4为kinstonusb了，而且还有个kinstonusb_link链接到它，以后我们就可用/dev/kinstonusb或/dev/kinstonusb_link来操作我们的优盘而不是/dev/sdc4了。

## 理解和认识udev[#](https://www.cnblogs.com/my-show-time/p/15108401.html#%E7%90%86%E8%A7%A3%E5%92%8C%E8%AE%A4%E8%AF%86udev)

因为本身从事存储行业，在工作中多次碰到用户有这样的要求：我的linux系统中原来有一块SCSI硬盘，系统分配的设备文件是/dev/sda。现在新增加了一个外置的磁盘阵列，通过SCSI卡连接。但接上这个磁盘阵列后，/dev/sda变成了磁盘阵列的硬盘了，原来内置的SCSI硬盘变成了 /dev/sdb，我希望将设备文件固定下来。  
 过去，我总是对用户说，这个比较麻烦，因为/dev/sda等文件都是linux内核自动分配的。很难固定下来，除非你更改加载SCSI卡驱动程序的顺序，让内置硬盘连接的SCSI卡比外接磁盘阵列连接的SCSI卡的驱动模块先加载到内核，这样就能保证/dev/sda总是指向内置的硬盘。但这种解决方法毕竟不太完美，而且对于其他的即插即用设备，如USB设备等都不适用。  
 近来，通过安装和升级linux-2.6内核，发现这个问题已经可以通过2.6内核新的sysfs文件系统和udev程序得到解决。下面就是我在学习了udev配置后的一点心得。我喜欢用FAQ的形式来说明。

问：什么是udev?  
 答：udev是一种工具，它能够根据系统中的硬件设备的状态动态更新设备文件，包括设备文件的创建，删除等。设备文件通常放在/dev目录下。使用udev后，在/dev目录下就只包含系统中真正存在的设备。

问：udev支持什么内核？  
 答：udev只支持linux-2.6内核，因为udev严重依赖于sysfs文件系统提供的信息，而sysfs文件系统只在linux-2.6内核中才有。

问：udev是一个内核程序还是用户程序？  
 答：udev是一个用户程序（user-mode daemon）。

问：udev和devfs有什么差别？  
 答：udev能够实现所有devfs实现的功能。但udev运行在用户模式中，而devfs运行在内核中。据称：devfs具有一些不太容易解决的先天缺陷。

问：udev的配置文件放在哪里？  
 答：udev是一个用户模式程序。它的配置文件是/etc/udev/udev.conf。这个文件一般缺省有这样几项：  
 udev_root="/dev" ; udev产生的设备文件的根目录是/dev  
 udev_db="/dev/.udevdb" ; 通过udev产生的设备文件形成的数据库  
 udev_rules="/etc/udev/rules.d" ;用于指导udev工作的规则所在目录。  
 udev_log="err" ;当出现错误时，用syslog记录错误信息。

问：udev的工作过程是怎样的？  
 答：由于没有研究过udev的源程序，不敢贸然就说udev的工作过程。我只是通过一些网上的资料和udev的说明文档，大致猜测它的工作过程可能是这样的。

1. 当内核检测到在系统中出现了新设备后，内核会在sysfs文件系统中为该新设备生成一项新的记录，一般sysfs文件系统会被 mount到 /sys目录中。新记录是以一个或多个文件或目录的方式来表示。每个文件都包含有特定的信息。（信息是如何表述的，还要另外研究？）
2. udev在系统中是以守护进程的方式udevd在运行，它通过某种途径（到底什么途径，目前还没搞懂。）检测到新设备的出现，通过查找设备对应的sysfs中的记录得到设备的一些信息。
3. udev 会根据/etc/udev/udev.conf文件中的udev_rules指定的目录，逐个检查该目录下的文件，这个目录下的文件都是针对某类或某个设备应该施行什么措施的规则文件。udev读取文件是按照文件名的ASCII字母顺序来读取的，如果udev一旦找到了与新加入的设备匹配的规则，udev 就会根据规则定义的措施对新设备进行配置。同时不再读后续的规则文件。

问：udev的规则文件的语法是怎样的？  
 答：udev的规则文件以行为单位，以"#"开头的行代表注释行。其余的每一行代表一个规则。每个规则分成一个或多个“匹配”和“赋值”部分。“匹配”部分用“匹配“专用的关键字来表示，相应的“赋值”部分用“赋值”专用的关键字来表示。“匹配”关键字包括：ACTION，KERNEL，BUS， SYSFS等等，“赋值”关键字包括：NAME，SYMLINK，OWNER等等。具体详细的描述可以阅读udev的man文档。

下面举个例子来说明一下，有这样一条规则：  
 SUBSYSTEM"net", ACTION"add", SYSFS{address}=="00:0d:87:f6:59:f3", IMPORT="/sbin/rename_netiface %k eth0"  
 这个规则中的“匹配”部分有三项，分别是SUBSYSTEM，ACTION和SYSFS。而"赋值"部分有一项，是IMPORT。这个规则就是说，当系统中出现的新硬件属于net子系统范畴，系统对该硬件采取的动作是加入这个硬件，且这个硬件在SYSFS文件系统中的“address”信息等于“00： 0d..."时，对这个硬件在udev层次施行的动作是调用外部程序/sbin/rename_netiface，传递的参数有两个，一个是“%k”，代表内核对该新设备定义的名称。另一个是”eth0“。  
 从上面这个例子中可以看出，udev的规则的写法比较灵活的，尤其在“匹配”部分中，可以通过诸如”*“, ”?“,[a-c],[1-9]等shell通配符来灵活匹配多个匹配项。具体的语法可以参考udev的man文档。

问：udev怎样做到不管设备连接的顺序而维持一个统一的设备名？  
 答：实际上，udev是通过对内核产生的设备名增加别名的方式来达到上述目的的。前面说过，udev是用户模式程序，不会更改内核的行为。因此，内核依然会我行我素地产生设备名如sda,sdb等。但是，udev可以根据设备的其他信息如总线（bus），生产商（vendor）等不同来区分不同的设备，并产生设备文件。udev只要为这个设备文件取一个固定的文件名就可以解决这个问题。在后续对设备的操作中，只要引用新的设备名就可以了。但为了保证最大限度的兼容，一般来说，新设备名总是作为一个对内核自动产生的设备名的符号链接（link）来使用的。

例如：内核产生了sda设备名，而根据信息，这个设备对应于是我的内置硬盘，那我就可以制定udev规则，让udev除了产生/dev/sda设备文件外，另外创建一个符号链接叫/dev/internalHD。这样，我在fstab文件中，就可以用/dev/internalHD来代替原来的 /dev/sda了。下次，由于某些原因，这个硬盘在内核中变成了sdb设备名了，那也不用着急，udev还会自动产生/dev/internalHD这个链接，并指向正确的/dev/sdb设备。所有其他的文件像fstab等都不用修改。

问：怎样才能找到这些设备信息，并把他们放到udev的规则文件中来匹配呢？  
 答：这个问题比较难，网上资料不多，我只找到一篇文章来介绍如何写udev的规则。他的基本方法是通过udevinfo这个实用程序来找到那些可以作为规则文件里的匹配项的项目。有这样两种情况可以使用这个工具：  
 第一种情况是，当你把设备插入系统后，系统为设备产生了设备名（如/dev/sda）。那样的话，你先用udevinfo -q path -n /dev/sda，命令会产生一个该设备名对应的在sysfs下的路径，如/block/sda。然后，你再用udevinfo -a -p /sys/block/sda，这个命令会显示一堆信息，信息分成很多块。这些信息实际来自于操作系统维护的sysfs链表，不同的块对应不同的路径。你就可以用这些信息来作为udev规则文件中的匹配项。但需要注意的是，同一个规则只能使用同一块中显示的信息，不能跨块书写规则。  
 第二种情况是，不知道系统产生的设备名，那就只有到/sys目录下去逐个目录查找了，反复用udevinfo -a -p /sys/path...这个命令看信息，如果对应的信息是这个设备的，那就恭喜你。否则就再换个目录。当然，在这种情况下，成功的可能性比较小。

## Udev (简体中文)

注意: 如果您是从DevFS升级到Udev, 请查看 DevFS to Udev.

这篇文档将介绍udev的一些新的变化。从084版开始，udev能够代替hotplug和coldplug的所有功能。正因为这样，hotplug包已经从Arch仓库中去掉了。

- 1 重要提示
- 2 基本需求
- 3 最近更新
- 4 模块禁用列表
- 5 load_modules: 有用的启动参数
- 6 已知的硬件问题
- 7 自动加载带来的一些问题  
     o 7.1 CPUFreq模块  
     　　　　o 7.2 声音问题和一些不能自动加载的模块  
     　　　　o 7.3 多个同类型设备（网卡，声卡）每次启动的都不同
- 8 自己编译内核造成的一些已知问题  
     o 8.1 Udev无法启动  
     　　　　o 8.2 CD/DVD符号和权限错误
- 9 Udev小窍门  
     o 9.1 自动加载usb设备

### 重要提示[#](https://www.cnblogs.com/my-show-time/p/15108401.html#%E9%87%8D%E8%A6%81%E6%8F%90%E7%A4%BA)

...切记，在使用udev加载任何modules（内核模块）之前（无论是否是启动时自动加载），您必须在/etc/rc.conf将MOD_AUTOLOAD选项设置为yes ，否则您必须手动加载这些modules。您可以修改rc.conf中的MODULES或者使用modprobe命令来手动加载您所需要的modules。另一种方法是用hwdetect --modules生成系统硬件的modules列表，然后将这个列表添加到rc.conf中让系统启动时自动加载这些modules。

### 基本需求[#](https://www.cnblogs.com/my-show-time/p/15108401.html#%E5%9F%BA%E6%9C%AC%E9%9C%80%E6%B1%82)

- 内核: 2.6.15或更高版本。
- 您将无法在fstab和bootloader设置中再使用DevFS格式的设备名称! 更多相关内容请查看DevFS to Udev。

### 最近更新[#](https://www.cnblogs.com/my-show-time/p/15108401.html#%E6%9C%80%E8%BF%91%E6%9B%B4%E6%96%B0)

- startudev程序被取出。如果需要重新加载udev规则请使用 /etc/start_udev。
- Udev代替了hotplug和hwdetect的功能。同时我们保存了hwdetect，并且只在mkinitrd程序生成initrd的时候用到。
- Udev可以同时加载多个模块，这样可以加快启动速度，然而，这样做的结果是她不能保证每次加载的顺序，所以当你使用多声卡或网卡的时候就会出现问题，这个问题下面将会再讨论。

### 模块禁用列表[#](https://www.cnblogs.com/my-show-time/p/15108401.html#%E6%A8%A1%E5%9D%97%E7%A6%81%E7%94%A8%E5%88%97%E8%A1%A8)

udev也会犯错或加载错误的模块。为了防止错误的发生，你可以使用模块禁用列表 。一旦加入该列表的模块，无论是启动时，或者时运行时（如usb硬盘等）udev都不会加载这些模块。

只需在您在 rc.conf的MODULES中对应模块前加上感叹号（!）就可禁用该模块。

例如,  
 MODULES=(!moduleA !moduleB)

load_modules: 有用的启动参数  
 如果您在内核启动参数中加入load_modules=off，那么udev会停止任何自动加载工作. 如果系统出现问题时，这个功能会十分有用。如果udev加载了有问题的模块导致系统挂起或者其它严重的问题时，你可以使用这个参数来禁用自动加载，以此来防止加载有问题的模块。

### 已知的硬件问题[#](https://www.cnblogs.com/my-show-time/p/15108401.html#%E5%B7%B2%E7%9F%A5%E7%9A%84%E7%A1%AC%E4%BB%B6%E9%97%AE%E9%A2%98)

- BusLogic设备被损坏而且导致启动时死机。

这是一个内核的Bug目前还没有修正。

- PCMCIA读卡器被认为是可移除设备.

把它们加入到/etc/pmount.allow中，使用hal的pmount来读取

自动加载带来的一些问题  
 CPUFreq模块

我门还没有找到一个很好的方法加载不同的CPUFreq控制器，所以我们把从自动加载进程里把它去掉了。如果您需要测量CPU频率，你必须在rc.conf的MODULES队列中显式的加入合适的模块。

声音问题和一些不能自动加载的模块  
 一些用户跟踪发现问题出在/etc/modprobe.conf中一些旧的部分，试着去掉这些旧的部分再试试看。  
 多个同类型设备（网卡，声卡）每次启动的都不同

因为udev同时加载所有模块，所以一些设备可能初始化顺序不同。例如同时有两个网卡时，它们总是在eth0和eth1之间变来变去。

常用的解决办法是在您的rc.conf文件中通过修改MODULES队列来指明顺序。这个队列里的模块将在udev自动加载之前由系统加载，因此您可以控制模块在启动时加载顺序。

### 在e100之前加载8139too

MODULES=(8139too e100)

另一个解决网卡的方法是使用udev-sanctified方法为每个网卡静态命名。创建文件/etc/udev/rules.d/10-network.rules然后将不同的网卡通过MAC地址绑定到不同的名字上：  
 SUBSYSTEM"net", SYSFS{address}"aa:bb:cc:dd:ee:ff", NAME="lan0"  
 SUBSYSTEM"net", SYSFS{address}"ff:ee:dd:cc:bb:aa", NAME="wlan0"

### 同时，您需要注意以下内容:[#](https://www.cnblogs.com/my-show-time/p/15108401.html#%E5%90%8C%E6%97%B6%E6%82%A8%E9%9C%80%E8%A6%81%E6%B3%A8%E6%84%8F%E4%BB%A5%E4%B8%8B%E5%86%85%E5%AE%B9)

- 您可以通过下面的命令获得网卡的MAC地址:: udevinfo -a -p /sys/class/net/<你的网卡>
- 注意在udev规则文件中使用小写的16进制MAC地址，因为udev无法识别大写的MAC地址。
- 一些用户在使用旧的命名方式时出现问题，例如: eth0, eth1, 等等. 如果出现这个问题，试试使用 "lan"或者"wlan"之类的名字.

注意不要忘记修改您的/dec/rc.conf和其它使用ethX命名的配置文件。

自己编译内核造成的一些已知问题  
 Udev无法启动

请确定您的内核版本大于或等于2.6.15。较早的内核没有udev自动装载所需要的uevent功能。

CD/DVD符号和权限错误

如果您使用2.6.15的内核的话，您需要安装ABS的uevent补丁（它从2.6.16内核中抽取了一些uevent功能）。您可以使用abs命令来同步ABS树，然后您就可以在/var/abs/kernels/kernel26/下找到abs补丁。

### Udev小窍门[#](https://www.cnblogs.com/my-show-time/p/15108401.html#udev%E5%B0%8F%E7%AA%8D%E9%97%A8)

自动加载usb设备

KERNEL=="sd[a-z]", NAME="%k", SYMLINK+="usb%m", GROUP="users", OPTIONS="last_rule"  
ACTION=="add", KERNEL=="sd[a-z][0-9]", SYMLINK+="usb%n", GROUP="users", NAME="%k"  
ACTION=="add", KERNEL=="sd[a-z][0-9]", RUN+="/bin/mkdir -p /mnt/usb%n"  
ACTION=="add", KERNEL=="sd[a-z][0-9]", PROGRAM=="/lib/udev/vol_id -t %N", RESULT=="vfat", RUN+="/bin/mount -t vfat -o rw,noauto,sync,dirsync,noexec,nodev,noatime,dmask=000,fmask=111 /dev/%k /mnt/usb%n", OPTIONS="last_rule"  
ACTION=="add", KERNEL=="sd[a-z][0-9]", RUN+="/bin/mount -t auto -o rw,noauto,sync,dirsync,noexec,nodev,noatime /dev/%k /mnt/usb%n", OPTIONS="last_rule"  
ACTION=="remove", KERNEL=="sd[a-z][0-9]", RUN+="/bin/umount -l /mnt/usb%n"  
ACTION=="remove", KERNEL=="sd[a-z][0-9]", RUN+="/bin/rmdir /mnt/usb%n", OPTIONS="last_rule"  
 

把这些udev规则放到/etc/udev/rules.d/下任何一个文件名以.rules结尾的文件中，例如/etc/udev/rules.d/sda.rules。

如果想同时建立/media到/mnt符号连接，可以使用下面的版本:

KERNEL=="sd[a-z]", NAME="%k", SYMLINK+="usbhd-%k", GROUP="users", OPTIONS="last_rule"  
ACTION=="add", KERNEL=="sd[a-z][0-9]", SYMLINK+="usbhd-%k", GROUP="users", NAME="%k"  
ACTION=="add", KERNEL=="sd[a-z][0-9]", RUN+="/bin/mkdir -p /media/usbhd-%k"  
ACTION=="add", KERNEL=="sd[a-z][0-9]", RUN+="/bin/ln -s /media/usbhd-%k /mnt/usbhd-%k"  
ACTION=="add", KERNEL=="sd[a-z][0-9]", PROGRAM=="/lib/udev/vol_id -t %N", RESULT=="vfat", RUN+="/bin/mount -t vfat -o rw,noauto,sync,dirsync,noexec,nodev,noatime,dmask=000,fmask=111 /dev/%k /media/usbhd-%k", OPTIONS="last_rule"  
ACTION=="add", KERNEL=="sd[a-z][0-9]", RUN+="/bin/mount -t auto -o rw,noauto,sync,dirsync,noexec,nodev,noatime /dev/%k /media/usbhd-%k", OPTIONS="last_rule"  
ACTION=="remove", KERNEL=="sd[a-z][0-9]", RUN+="/bin/rm -f /mnt/usbhd-%k"  
ACTION=="remove", KERNEL=="sd[a-z][0-9]", RUN+="/bin/umount -l /media/usbhd-%k"  
ACTION=="remove", KERNEL=="sd[a-z][0-9]", RUN+="/bin/rmdir /media/usbhd-%k", OPTIONS="last_rule"  


注意！ 如果你是用的其它的固定设备（例如SATA的硬盘，您可以从/etc/fstab中查看）被识别为/dev/sdX，您必须从sd[a-z] 中去掉你的那个sdX。例如，如果您的SATA硬盘被是识别为/dev/sda，您就需要把所有的“sd[a-z]”替换成“sd[b-z]”。在规则文件的文件名前加上数字（如:010.udev.rules）是个很好的主意，这样udev在读取标准规则前，将会读取这个规则文件。这些规则设置后不需要修改/etc/fstab文件。请查看mount命令的参数来修改权限等特性（您可以从论坛搜索查看mount命令的参数，然后根据您的需要修改它们）。
