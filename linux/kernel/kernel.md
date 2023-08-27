# kernel

## source 安装
```shell
# centos
yum install kernel-devel

# ubuntu
sudo apt-get install kernel-source-xxxx
```

## 生成 compile_commands.json
```shell
scripts/clang-tools/gen_compile_commands.py
```

- 清理全部 make 的产物
```shell
make mrproper
```

- 更换内核
```shell
git clone [https://github.com/pimlie/ubuntu-mainline-kernel.sh.git](https://github.com/pimlie/ubuntu-mainline-kernel.sh.git)

cd ubuntu-mainline-kernel.sh/

sudo mv ubuntu-mainline-kernel.sh /usr/local/bin/

sudo ubuntu-mainline-kernel.sh -i v5.11.0#下载5.11.0版本内核，可指定其他版本

sudo ubuntu-mainline-kernel.sh -u #删除不需要的版本，这样就可以留下需要版本，实现版本随意升级甚至降级

重启

1.安装需要的内核版本：

apt install linux-image-5.4.0-1029-aws

2.列出可用的ubuntu内核版本：

grep 'menuentry \|submenu ' /boot/grub/grub.cfg | cut -f2 -d "'"

3.备份一下grub文件：

cp /etc/default/grub /etc/default/grub.bak

4.修改内核版本：

vim /etc/default/grub

GRUB_DEFAULT='Advanced options for Ubuntu>Ubuntu, with Linux 5.4.0-1029-aws'

#根据上面可用的内核版本进行设置

update-grub 

#更新

5.重启！之后uname -r 查看当前内核版本。
```



# 编译Linux后在根目录下生成的几个文件详解
- modules.builtin
- modules.builtin.modinfo
- modules-only.symvers
- modules.order
- Module.symvers
- System.map
- vmlinux
- vmlinux.o
- vmlinux.symvers

## modules.builtin
modules.builtin 是构建 Linux 内核时生成的文件
它包含一系列内置内核模块
这些模块被编译到内核映像中并成为内核本身的一部分
而不是在运行时作为单独的模块加载。

构建内核时，make 命令会编译所有必需的内核模块并创建内核映像文件
modules.builtin 文件是在此过程中创建的
它包含已编译到内核映像中的模块列表。

modules.builtin 文件通常位于系统根文件系统的 /lib/modules/[kernel version]/ 目录中
它列出了所有内置内核模块的名称及其依赖项。

内核在启动时使用 modules.builtin 文件来加载内置模块并确保它们对系统可用
由于模块内置在内核镜像中，运行时不需要单独加载，可以提高系统性能，降低管理内核模块的复杂度。

modules.builtin 文件的内容可用于调试内核模块加载问题或了解内置内核模块之间的依赖关系
然而，在大多数情况下，该文件不需要手动编辑或修改，因为它是由内核构建系统自动生成的。

## modules.builtin.modinfo
modules.builtin.modinfo 是在使用内置模块构建 Linux 内核时生成的文件
它包含有关编译到内核映像中的内置模块的元数据信息。

当使用内置模块构建内核时，make 命令会编译所有必需的内核模块并创建内核映像文件
modules.builtin.modinfo 文件是在此过程中创建的，它包含有关内置模块的元数据信息，例如模块名称、版本、作者和描述。

modules.builtin.modinfo 文件通常位于系统根文件系统的 /lib/modules/[kernel version]/ 目录中，旁边是 modules.builtin 文件
它列出了所有内置内核模块的元数据信息。

内核在启动时使用 modules.builtin.modinfo 文件来加载内置模块并确保它们对系统可用
因为模块内置于内核镜像中，所以它们的元数据信息也包含在镜像中，允许内核识别和加载模块而不需要单独的模块文件。

modules.builtin.modinfo 文件的内容可用于调试内核模块加载问题或了解内置内核模块的元数据信息
然而，在大多数情况下，该文件不需要手动编辑或修改，因为它是由内核构建系统自动生成的。

## modules-only.symvers
modules-only.symvers 是在使用内置模块构建 Linux 内核时生成的文件
它包含一个符号列表（函数、变量等），这些符号由内置模块导出，可以被其他模块或内核本身使用。

当使用内置模块构建内核时，make 命令会编译所有必需的内核模块并创建内核映像文件
modules-only.symvers 文件是在此过程中创建的，它包含由内置模块导出的符号列表。

modules-only.symvers 文件通常位于系统根文件系统的 /usr/src/[kernel version]/ 目录中
它列出了所有内置内核模块的符号。

内核构建系统使用 modules-only.symvers 文件来确保在构建依赖于内置模块导出的符号的外部内核模块时所有必要的符号都可用
使用此文件代替构建外部内核模块时生成的 Module.symvers 文件，因为内置模块的符号在内核映像中已经可用，不需要单独导出。

modules-only.symvers 文件的内容可用于调试内核模块构建问题或了解内置内核模块与外部内核模块之间的依赖关系
然而，在大多数情况下，该文件不需要手动编辑或修改，因为它是由内核构建系统自动生成的。

## modules.order
modules.order 是编译 Linux 内核模块时生成的文件。 它包含系统启动时需要按特定顺序加载的模块列表。

当系统启动时，内核加载所有必要的内核模块来初始化系统的硬件和软件组件
这些模块通常以特定顺序加载，以确保满足所有依赖关系并且系统可以正常运行。

modules.order 文件是在编译内核模块时由 make 命令生成的
它与模块的源代码和 Makefile 在同一目录中生成。 该文件名为“modules.order”，包含按加载顺序排列的模块列表。

modules.order 文件的内容可用于调试内核模块加载问题或了解内核模块之间的依赖关系
但是，在大多数情况下，该文件不需要手动编辑或修改，因为它是由构建系统自动生成的。

值得注意的是，并非所有 Linux 发行版都使用 modules.order 文件
一些发行版使用其他机制来管理内核模块加载，例如 systemd 或 udev
在这些情况下，modules.order 文件可能不存在或可能被系统忽略。

## Module.symvers
Module.symvers 是构建外部内核模块时生成的文件
它包含一个符号列表（函数、变量等），这些符号由模块导出并且可以被其他模块或内核本身使用。

构建外部内核模块时，make 命令会编译模块并创建一个二进制文件，该文件可以在运行时加载到内核中
Module.symvers 文件是在此过程中创建的，它包含模块导出的符号列表。

Module.symvers 文件通常与模块的源代码和 Makefile 位于同一目录中。 它列出了模块的符号，以及它们的地址和版本信息。

内核构建系统使用 Module.symvers 文件来确保在构建依赖于模块导出的符号的其他外部内核模块时所有必要的符号都可用
该文件还由内核在运行时使用，以确保在将模块加载到内核中时符号可用。

Module.symvers 文件的内容可用于调试内核模块构建问题或了解外部内核模块与内置内核模块之间的依赖关系
然而，在大多数情况下，该文件不需要手动编辑或修改，因为它是由内核构建系统自动生成的。

## System.map
System.map 是构建 Linux 内核时生成的文件
它包含在内核中定义的符号（函数、变量等）列表及其在内存中的相应地址。

构建内核时，make 命令会编译所有必需的源代码文件并创建内核映像文件
System.map 文件是在此过程中创建的，它包含一个在内核中定义的符号列表，以及它们在内存中的地址。

System.map 文件通常位于系统根文件系统的 /boot/ 目录中。 它列出了内核的符号，包括内置内核模块及其符号。

内核在运行时使用 System.map 文件将符号名称映射到它们相应的内存地址
内核调试器和其他系统工具使用此信息来分析内核崩溃和其他问题。

System.map 文件的内容可用于调试内核问题或了解内核的内存布局
然而，在大多数情况下，该文件不需要手动编辑或修改，因为它是由内核构建系统自动生成的。

## vmlinux
vmlinux 是一个包含未压缩 Linux 内核的文件
它在内核构建过程中生成，通常位于系统根文件系统的 /boot/ 目录中。

vmlinux 文件是系统引导时加载到内存中的内核的原始二进制映像
它包含运行内核所需的所有内核代码、数据和符号。

vmlinux 文件主要用于调试内核问题，因为它包含分析内核崩溃和其他问题所需的所有信息
该文件可以加载到内核调试器中，例如 gdb，以分析内核的行为。

在大多数情况下，正常系统操作不需要 vmlinux 文件，可以安全地删除或移除
但是，建议保留该文件的副本以备调试之需。

在内核构建过程中，vmlinux 文件被压缩成一个较小的映像文件，通常带有 .gz 或 .xz 扩展名，这是在启动时加载的实际内核映像。

## vmlinux.o
vmlinux.o 是在内核构建过程中生成的文件
它是一个中间目标文件，包含了 Linux 内核的所有编译代码和数据，但它还没有链接到最终的内核映像中。

vmlinux.o 文件由编译器生成，并用作链接器的输入以创建最终内核映像，通常命名为 vmlinux
链接器将其他目标文件（例如内置内核模块）添加到 vmlinux.o 文件以创建最终映像。

vmlinux.o 文件包含分析内核所需的所有符号和调试信息，但它本身不可执行
它主要用于调试内核问题和了解内核的行为。

在大多数情况下，正常系统操作不需要 vmlinux.o 文件，可以安全地删除或移除
但是，建议保留该文件的副本以备调试之需。

vmlinux.o 文件通常位于与内核源代码和 Makefile 相同的目录中，在构建内核的目录中。

## vmlinux.symvers
vmlinux.symvers 是在内核构建过程中生成的文件
它包含一个符号列表（函数、变量等），这些符号在内核中定义并且可以被外部内核模块使用。

构建内核时，make 命令会编译所有必需的源代码文件，包括内置内核模块，并创建内核映像文件
vmlinux.symvers 文件是在此过程中创建的，它包含内核中定义的符号列表及其版本。

vmlinux.symvers 文件通常与内核源代码和 Makefile 位于构建内核的目录中
它列出了内核的符号，包括内置内核模块及其符号。

vmlinux.symvers 文件由外部内核模块使用，以确保在模块加载到内核中时它们所需的符号可用
内核构建系统也使用该文件来确保在构建依赖于内核中定义的符号的外部内核模块时所有必要的符号都可用。

vmlinux.symvers 文件的内容可用于调试内核模块构建问题或了解外部内核模块与内置内核模块之间的依赖关系
然而，在大多数情况下，该文件不需要手动编辑或修改，因为它是由内核构建系统自动生成的。
