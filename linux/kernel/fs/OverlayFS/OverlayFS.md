# OverlayFS

挂载选项支持(即"-o"参数):
1. lowerdir=xxx：指定用户需要挂载的lower层目录（支持多lower，最大支持500层）；
2. upperdir=xxx：指定用户需要挂载的upper层目录；
3. workdir=xxx：指定文件系统的工作基础目录，挂载后内容会被清空，且在使用过程中其内容用户不可见；
4. default_permissions：功能未使用；
5. redirect_dir=on/off：开启或关闭redirect directory特性，开启后可支持merged目录和纯lower层目录的rename/renameat系统调用；
6. index=on/off：开启或关闭index特性，开启后可避免hardlink copyup broken问题。

```shell
mount -t overlay overlay -o lowerdir=lower1:lower2:lower3,upperdir=upper,workdir=work merged

<!-- 其中"lower1:lower2:lower3"表示不同的lower层目录，不同的目录使用":"分隔，层次关系依次为lower1 > lower2 > lower3 -->
```

```shell
cd /tmp/
mkdir overlay/
cd overlay/
mkdir lower upper merged work
tree
.
├── lower
│   ├── ld
│   │   └── ld.txt
│   └── ld1.txt
├── merged
├── upper
│   ├── ud
│   │   └── ud.txt
│   └── ud2.txt
└── work
 
6 directories, 4 files
 
cat lower/ld1.txt
ld 1 test

cat upper/ud2.txt
ud 2 test

sudo mount -t overlay overlay -olowerdir=./lower,upperdir=./upper,workdir=./work ./merged

tree
.
├── lower
│   ├── ld
│   │   └── ld.txt
│   └── ld1.txt
├── merged
│   ├── ld
│   │   └── ld.txt
│   ├── ld1.txt
│   ├── ud
│   │   └── ud.txt
│   └── ud2.txt
├── upper
│   ├── ud
│   │   └── ud.txt
│   └── ud2.txt
└── work
    └── work [error opening dir]
 
9 directories, 8 files

文件名及目录不相同，则 lower 及 upper 目录中的文件及目录按原结构都融入到 merged 目录中；
文件名相同，只显示 upper 层的文件，而 lower的隐藏 ;
目录名相同， 对目录进行合并成一个目录。将目录及目录下的所有文件合并到 merged 的 dir目录，目录内如有文件名相同，则同样只显示 upper 的。
overlay只支持两层，upper文件系统通常是可写的；lower文件系统则是只读，这就表示着，当我们对 overlay 文件系统做任何的变更，都只会修改 upper 文件系统中的文件。那下面看一下overlay文件系统的读，写，删除操作。

[读]:
读 upper 没有而 lower 有的文件时，需从 lower 读；
读只在 upper 有的文件时，则直接从 upper 读；
读 lower 和 upper 都有的文件时，则直接从 upper 读。

[写]:
对只在 upper 有的文件时，则直接在 upper 写；
对在lower 和 upper 都有的文件时，则直接在 upper 写；
对只在 lower 有的文件写时，则会做一个copy_up 的操作，先从 lower将文件拷贝一份到upper，同时为文件创建一个硬链接。此时可以看到 upper 目录下生成了两个新文件，写的操作只对从lower 复制到 upper 的文件生效，而 lower 还是原文件。

[删]:
删除 lower 和 upper 都有的文件时，upper 的会被删除，在 upper 目录下创建一个 ‘without' 文件，而 lower 的不会被删除；
删除 lower 有而 upper 没有的文件时，会为被删除的文件在 upper 目录下创建一个 ‘without' 文件，而 lower 的不会被删除；
删除 lower 和 upper 都有的目录时，upper 的会被删除，在 upper 目录下创建一个类似‘without' 文件的  ‘opaque' 目录，而 lower 的不会被删除。

```
