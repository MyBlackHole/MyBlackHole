exportfs(8)                         System Manager's Manual                            exportfs(8)

NAME
       exportfs - 管理维护NFS导出表

SYNOPSIS
       /usr/sbin/exportfs [-avi] [-o options,..] [client:/path ..]
       /usr/sbin/exportfs -r [-v]
       /usr/sbin/exportfs [-av] -u [client:/path ..]
       /usr/sbin/exportfs [-v]
       /usr/sbin/exportfs -f
       /usr/sbin/exportfs -s

DESCRIPTION
       
       NFS服务端会维护一张可被NFS客户端访问的本地物理文件系统的表。表中的每个文件系统都被称为
       导出的文件系统，或简称为导出项。

       exportfs命令维护NFS服务端当前导出表。其中导出主表存放在/var/lib/nfs/etab文件中。当客户端
       发送一个NFS MOUNT请求时，rpc.mountd进程会读取该文件。

       一般来说，导出主表是"exportfs -s"读取/etc/exports和/etc/exports.d/*.exports文件来初始化的。
       但是，系统管理员可以使用exportfs命令直接向主表中添加或删除导出项，而不需要去修改
       /etc/exports或/etc/exports.d/*.exports文件。

       exportfs和它的搭档程序rpc.mountd以两种模式之一工作：传统模式用于Linux Kernel 2.4以及之前
       的版本，新模式应用于内核2.6和之后的版本，新模式提供了nfsd虚拟文件系统，并将它们挂载在
       /proc/fs/nfsd或/proc/fs/nfs上。在kernel 2.6之后，如果未挂载nfsd虚拟文件系统，则表示工作在
       传统模式下。

       在新模式下，exportfs不会给内核任何信息，而是通过文件/var/lib/nfs/etab将信息交给rpc.mountd，
       然后rpc.mountd就可以按需管理关于导出信息的内核请求。

       传统模式下，exports文件只能识别主机，不能识别网段和网络组，且会直接将导出信息交给内核中
       的导出表，同时写入到文件/var/lib/nfs/etab文件中。

OPTIONS
       -d kind  or  --debug kind
              开启调试功能。有效的kind值为：all, auth, call, general和parse。

       -a     导出或卸载所有目录。

       -o options,...
              指定一系列导出选项(如rw,async,root_squash)，这些导出选项在exports(5)的man文档中有记录。

       -i     忽略/etc/exports和/etc/exports.d目录下文件。此时只有命令行中给定选项和默认选项会生效。

       -r     重新导出所有目录，并同步修改/var/lib/nfs/etab文件中关于/etc/exports和/etc/exports.d/
              *.exports的信息(即还会重新导出/etc/exports和/etc/exports.d/*等导出配置文件中的项)。该
              选项会移除/var/lib/nfs/etab中已经被删除和无效的导出项。

       -u     卸载(即不再导出)一个或多个导出目录。

       -f     如果/prof/fs/nfsd或/proc/fs/nfs已被挂载，即工作在新模式下，该选项将清空内核中导出表中
              的所有导出项。客户端下一次请求挂载导出项时会通过rpc.mountd将其添加到内核的导出表中。

       -v     输出详细信息。

       -s     显示适用于/etc/exports的当前导出目录列表。

DISCUSSION
   Exporting Directories
       synopsis中的第一项显示了当要向导出表中添加导出条目时如何调用exportfs命令。当使用"exportfs -a"
       时，所有记录在/etc/exports和/etc/exports.d/*.exports中的导出项都会被添加到文件/var/lib/nfs/etab
       中。且会按需更新内核中的导出表。

       client:/path参数中的/path指定了本地需要导出的目录，而client表示可以访问该导出目录的客户端。
       关于导出选项以及client的指定格式，则参见exports(5)的man文档。

       IPv6地址中包含冒号，但冒号已经用于分隔"client"和"/path"命令行参数。因此当使用IPv6地址指定客
       户端时，需要将该地址使用中括号包围。对于IPv6网络地址，将前缀放在关闭括号后面。
       (如：[X:X:X:X:X:X:X:X]:/path和[fe80::]/64:/path)

       如果要将目录导出为所有人可访问(即导出给整个world)，则只需使用":/path"即可。但此时可能会给出
       警告信息，可以使用

       一个特定的host/directory的导出选项可以有多个来源。默认导出选项是sync,ro,root_squash,wdelay。
       这些默认选项可以被/etc/exports或/etc/exports.d/*.exports中的选项覆盖掉。

       系统管理员可以使用"exportfs -o options"来覆盖所有其他来源的选项。命令行中指定导出选项时使用
       逗号分隔导出选项列表。也就是说，exportfs可用于修改已导出目录的导出选项。

   Unexporting Directories
       synopsis中的第三项显示了如何卸载当前已导出的目录。当使用"exportfs -ua"时，/var/lib/nfs/etab中
       的所有项都将从内核导出表中移除，且该文件会被清空。这对要关闭所有NFS活动时很有效。

       如果只要移除某一个导出项，则指定client:/path即可。它会删除/var/lib/nfs/etab中对应的项，并从
       内核导出表中移除对应的项。

   Dumping the Export Table
       当exportfs命令不接任何选项时，将输出当前所有已导出目录的列表。使用"-v"选项可输出更详细信息。

EXAMPLES
       以下示例会导出/etc/exports和/etc/exports.d/*.exports中的目录，且会记录到/var/lib/nfs/etab中，
       并最终会记录到内核导出表中：

       # exportfs -a

       导出目录/usr/tmp给django主机，且允许客户端发送不安全的文件锁请求：

       # exportfs -o insecure_locks django:/usr/tmp

       卸载/usr/tmp目录：

       # exportfs -u django:/usr/tmp

       卸载所有导出列表：

       # exportfs -au

       导出/usr/tmp目录给IPv6的本地客户端：

       # exportfs [fe80::]/64:/usr/tmp

USAGE NOTES
       Exporting to IP networks or DNS and NIS domains does not enable clients from these groups to 
       access NFS immediately.  Rather, these sorts of  exports are hints to rpc.mountd(8) to grant
       any mount requests from these clients.  This is usually not a problem, because any existing 
       mounts are preserved in rmtab across reboots.

       When unexporting a network or domain entry, any current exports to members of this group wi-
       ll be checked against the remaining valid exports and if they themselves are no longer valid
       they will be removed.

FILES
       /etc/exports             input file listing exports, export options, and access control lists

       /etc/exports.d           directory where extra input files are stored.  
                                Note: only files that end with .exports are used.

       /var/lib/nfs/etab        master table of exports

       /var/lib/nfs/rmtab       table of clients accessing server's exports

SEE ALSO
       exports(5), rpc.mountd(8), netgroup(5)

AUTHORS
       Olaf Kirch <okir@monad.swb.de>
       Neil Brown <neilb@cse.unsw.edu.au>
