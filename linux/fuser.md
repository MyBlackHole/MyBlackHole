# fuser 

## 安装
- centos
```shell
yum install psmisc
```

## 语法
**fuser(选项)(参数)**
| 选项    | 说明           |
| ------- | -------------- |
| c | 代表当前目录   |
| e | 将此文件作为程序的可执行对象使用   |
| f | 打开的文件。默认不显示   |
| F | 打开的文件，用于写操作。默认不显示。      |
| r | 指示该目录为进程的根目录|
| m | 指示进程使用该文件进行内存映射，抑或该文件为共享库文件，被进程映射进内存|
| s | 将此文件作为共享库（或其他可装载对象）使用|

| 参数    | 说明           |
| ------- | -------------- |
|-c| 和-m一样，用于POSIX兼容|
|-k| 杀掉访问文件的进程。如果没有指定-signal就会发送SIGKILL信号|
|-i| 杀掉进程之前询问用户，如果没有-k这个选项会被忽略|
|-l| 列出所有已知的信号名称|
|-m| name 指定一个挂载文件系统上的文件或者被挂载的块设备（名称name）。这样所有访问这个文件或者文件系统的进程都会被列出来。如果指定的是一个目录会自动转换成"name/",并使用所有挂载在那个目录下面的文件系统|
|-n| space 指定一个不同的命名空间(space).这里支持不同的空间文件(文件名，此处默认)、tcp(本地tcp端口)、udp(本地udp端口)。对于端口， 可以指定端口号或者名称，如果不会引起歧义那么可以使用简单表示的形式，例如：name/space (即形如:80/tcp之类的表示)|
|-s| 静默模式，这时候-u,-v会被忽略。-a不能和-s一起使用|
|-signal|使用指定的信号，而不是用SIGKILL来杀掉进程。可以通过名称或者号码来表示信号(例如-HUP,-1),这个选项要和-k一起使用，否则会被忽略|
|-u|在每个PID后面添加进程拥有者的用户名称|
|-v|详细模式。输出似ps命令的输出，包含PID,USER,COMMAND等许多域,如果是内核访问的那么PID为kernel.  -V 输出版本号|
|-4|使用IPV4套接字,不能和-6一起应用，只在-n的tcp和udp的命名存在时不被忽略|
|-6|使用IPV6套接字,不能和-4一起应用，只在-n的tcp和udp的命名存在时不被忽略|
|-| 重置所有的选项，把信号设置为SIGKILL|

### 例子
| 命令                    | 说明                           |
| -------------------- | --------------------------- |
| fuser -u -m /media/black/Data  | 查看指定挂载的文件系统被什么进程与用户占用  |
| fuser -um /dev/sda2 |显示使用某个文件的进程信息  |
| fuser -m -k -i readme  |杀掉打开readme文件的程序  |
| fuser -v -n tcp 80 或 $fuser -v 80/tcp    |查看那些程序使用tcp的80端口  |
| fuser -l |fuser不同信号的应用   |


- 查询正在占有的程序
```shell
fuser -mv /aiopool/
```

## error

- fusermount: option allow_other only allowed if 'user_allow_other' is set in /etc/fuse.conf
```shell
sudo nvim /etc/fuse.conf
# add user_allow_other
user_allow_other

或

sudo chmod 666 /etc/fuse.conf

或

addgroup <username> fuse
```
