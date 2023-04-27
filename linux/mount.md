### 描述
**文件挂载(重启失效)**
### 语法
```
-V  显示程序版本 
-h  显示辅助讯息 
-v  显示较讯息，通常和 -f 用来除错。 
-a  将 /etc/fstab 中定义的所有档案系统挂上。 
-F  这个命令通常和 -a 一起使用，它会为每一个 mount 的动作产生一个行程负责执行。在系统需要挂上大量 NFS 档案系统时可以加快挂上的动作。 
-f  通常用在除错的用途。它会使 mount 并不执行实际挂上的动作，而是模拟整个挂上的过程。通常会和 -v 一起使用。 
-n  一般而言，mount 在挂上后会在 /etc/mtab 中写入一笔资料。但在系统中没有可写入档案系统存在的情况下可以用这个选项取消这个动作。 
-s-r  等于 -o ro 
-w  等于 -o rw 
-L  将含有特定标签的硬盘分割挂上。 
-U  将档案分割序号为 的档案系统挂下。-L 和 -U 必须在/proc/partition 这种档案存在时才有意义。 
-t  指定档案系统的型态，通常不必指定。mount 会自动选择正确的型态。 
-o async  打开非同步模式，所有的档案读写动作都会用非同步模式执行。 
-o sync  在同步模式下执行。 
-o atime、-o noatime   当 atime 打开时，系统会在每次读取档案时更新档案的『上一次调用时间』。当我们使用 flash 档案系统时可能会选项把这个选项关闭以减少写入的次数。 
-o auto、-o noauto  打开/关闭自动挂上模式。 
-o defaults  使用预设的选项 rw, suid, dev, exec, auto, nouser, and async. 
-o dev、-o nodev-o exec、-o noexec  允许执行档被执行。 
-o suid、-o nosuid  允许执行档在 root 权限下执行。 
-o user、-o nouser  使用者可以执行 mount/umount 的动作。 
-o remount  将一个已经挂下的档案系统重新用不同的方式挂上。例如原先是唯读的系统，现在用可读写的模式重新挂上。 
-o ro  用唯读模式挂上。 
-o rw  用可读写模式挂上。 
-o loop=  使用 loop 模式用来将一个档案当成硬盘分割挂上系统。 
```
### 文件系统
```
overlay  
    lowerdir=<dir>：指定用户需要挂载的lower层目录，lower层支持多个目录，用“:”间隔，优先级依次降低。最多支持500层。 
    upperdir=<dir>：指定用户需要挂载的upper层目录，upper层优先级高于所有的lower层目录。 
    workdir=<dir>：指定文件系统挂载后用于存放临时和间接文件的工作基础目录。 
    default_permissions： 
    redirect_dir=on/off：开启或关闭redirect directory特性，开启后可支持merged目录和纯lower层目录的rename/renameat系统调用。 
    index=on/off：开启或关闭index特性，开启后可避免hardlink copyup broken问题。 

tmpfs  

nfs  

cifs  

minix 

```
### 例子
```shell
如出现只读错误
    umount /dev/sda1  #（出现目标正忙就把文件系统先该成只读）
    sudo ntfsfix /dev/sda1 # 修复文件系统 
    mount -t nfs 192.168.31.5:/medis/heidong/shijian ~/Documents/shijian # 挂载NFS文件服务器的文件 

mount -o remount, rw /etc # 挂载当前分区为读写rw 
mount -o remount, ro /etc  # 挂载只读ro 
mount -t cifs //192.168.31.5/e /media/heidong/guazai/  # 挂载共享文件到本地 
Mount -t  cifs //192.168.2.246/e /media/black/w -o username=BH  # 使用BH账号挂载共享文件到本地 
sudo mount -t cifs -o username=Everyone //192.168.199.123/win  /home/sk/win    
mount -t nfs 192.168.1.2:/tmp/share /usr/tomcat/here  # 挂载nfs文件系统共享目录 
mount -o remount /dev/shm  # 重新挂载 
mount -t tmpfs -o size=1G tmpfs /mnt/mytmpfs  # 挂载内存文件系统 
mount -t overlay -o lowerdir=lower,upperdir=upper,workdir=work overlay merge  # 将lower和upper进行overlay，挂载到merge目录，临时workdir为work目录 
mount -t overlay -o lowerdir=upper:lower overlay merge  # 如下同样将lower和upper进行overlay到merge，但是merge为只读属性。 
mount -t minix -o loop,offset=1024 $OSLAB_PATH/hdc-0.11.img $OSLAB_PATH/hdc  # 挂载minix 
```