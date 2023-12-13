# configure

- 静态编译
```shell
configure LDFLAGS=-static
```

<!-- enable-shared 动态 -->
<!-- enable-optimizations 编译优化 -->
<!-- with-openssl 设置自定义 openssl 安装路径 -->
<!-- --enable-shared=no --enable-static=yes （静态） -->
<!-- --enable-shared=yes --enable-static=no （动态） -->
```shell
--cache-file=FILE
'configure'会在你的系统上测试存在的特性(或者bug!)。为了加速随后进行的配置，测试的结果会存储在一个cache file里。当configure一个每个子树里都有'configure'脚本的复杂的源码树时，一个很好的cache file的存在会有很大帮助。

--help
输出帮助信息。即使是有经验的用户也偶尔需要使用使用'--help'选项，因为一个复杂的项目会包含附加的选项。例如，GCC包里的'configure'脚本就包含了允许你控制是否生成和在GCC中使用GNU汇编器的选项。


--no-create
'configure'中的一个主要函数会制作输出文件。此选项阻止'configure'生成这个文件。你可以认为这是一种演习(dry run)，尽管缓存(cache)仍然被改写了。


--quiet

--silent
当'configure'进行他的测试时，会输出简要的信息来告诉用户正在作什么。这样作是因为'configure'可能会比较慢，没有这种输出的话用户将会被扔在一旁疑惑正在发生什么，使用这两个选项中的任何一个都会把你扔到一旁。(译注：这两句话比较有意思，原文是这样的：If there was no such output, the user would be left wondering what is happening. By using this option, you too can be left wondering!)


--version
打印用来产生'configure'脚本的Autoconf的版本号。


--prefix=PEWFIX
'--prefix'是最常用的选项。制作出的'Makefile'会查看随此选项传递的参数，当一个包在安装时可以彻底的重新安置他的结构独立部分。举一个例子，当安装一个包，例如说Emacs，下面的命令将会使Emacs Lisp file被安装到"/opt/gnu/share"：
$ ./configure --prefix=/opt/gnu


--exec-prefix=EPREFIX
与'--prefix'选项类似，但是他是用来设置结构倚赖的文件的安装位置，编译好的'emacs'二进制文件就是这样一个问件。如果没有设置这个选项的话，默认使用的选项值将被设为和'--prefix'选项值一样。


--bindir=DIR
指定二进制文件的安装位置，这里的二进制文件定义为可以被用户直接执行的程序。


--sbindir=DIR
指定超级二进制文件的安装位置。这是一些通常只能由超级用户执行的程序。


--libexecdir=DIR
指定可执行支持文件的安装位置。与二进制文件相反，这些文件从来不直接由用户执行，但是可以被上面提到的二进制文件所执行。


--datadir=DIR
指定通用数据文件的安装位置。


--sysconfdir=DIR
指定在单个机器上使用的只读数据的安装位置。


--sharedstatedir=DIR
指定可以在多个机器上共享的可写数据的安装位置。


--localstatedir=DIR
指定只能单机使用的可写数据的安装位置。

--libdir=DIR
指定库文件的安装位置。


--includedir=DIR
指定C头文件的安装位置。其他语言如C++的头文件也可以使用此选项。


--oldincludedir=DIR
指定为除GCC外编译器安装的C头文件的安装位置。


--infodir=DIR
指定Info格式文档的安装位置.Info是被GNU工程所使用的文档格式。


--mandir=DIR
指定手册页的安装位置。


--srcdir=DIR
这个选项对安装没有作用，他会告诉'configure'源码的位置。一般来说不用指定此选项，因为'configure'脚本一般和源码文件在同一个目录下。


--program-prefix=PREFIX
指定将被加到所安装程序的名字上的前缀。例如，使用'--program-prefix=g'来configure一个名为'tar'的程序将会使安装的程序被命名为'gtar'。当和其他的安装选项一起使用时，这个选项只有当他被`Makefile.in'文件使用时才会工作。


--program-suffix=SUFFIX
指定将被加到所安装程序的名字上的后缀。


--program-transform-name=PROGRAM
这里的PROGRAM是一个sed脚本。当一个程序被安装时，他的名字将经过`sed -e PROGRAM'来产生安装的名字。


--build=BUILD
指定软件包安装的系统平台。如果没有指定，默认值将是'--host'选项的值。


--host=HOST
指定软件运行的系统平台。如果没有指定。将会运行`config.guess'来检测。


--target=GARGET
指定软件面向(target to)的系统平台。这主要在程序语言工具如编译器和汇编器上下文中起作用。如果没有指定，默认将使用'--host'选项的值。


--disable-FEATURE
一些软件包可以选择这个选项来提供为大型选项的编译时配置，例如使用Kerberos认证系统或者一个实验性的编译器最优配置。如果默认是提供这些特性，可以使用'--disable-FEATURE'来禁用它，这里'FEATURE'是特性的名字，例如：

$ ./configure --disable-gui


-enable-FEATURE[=ARG]
相反的，一些软件包可能提供了一些默认被禁止的特性,可以使用'--enable-FEATURE'来起用它。这里'FEATURE'是特性的名字。一个特性可能会接受一个可选的参数。例如：

$ ./configure --enable-buffers=128

`--enable-FEATURE=no'与上面提到的'--disable-FEATURE'是同义的。


--with-PACKAGE[=ARG]
在自由软件社区里，有使用已有软件包和库的优秀传统。当用'configure'来配置一个源码树时，可以提供其他已经安装的软件包的信息。例如，倚赖于Tcl和Tk的BLT器件工具包。要配置BLT，可能需要给'configure'提供一些关于我们把Tcl和Tk装的何处的信息：

$ ./configure --with-tcl=/usr/local --with-tk=/usr/local
'--with-PACKAGE=no'与下面将提到的'--without-PACKAGE'是同义的。


--without-PACKAGE
有时候你可能不想让你的软件包与系统已有的软件包交互。例如，你可能不想让你的新编译器使用GNU ld。通过使用这个选项可以做到这一点：

$ ./configure --without-gnu-ld

--x-includes=DIR
这个选项是'--with-PACKAGE'选项的一个特例。在Autoconf最初被开发出来时，流行使用'configure'来作为Imake的一个变通方法来制作运行于X的软件。'--x-includes'选项提供了向'configure'脚本指明包含X11头文件的目录的方法。


--x-libraries=DIR
类似的，'--x-libraries'选项提供了向'configure'脚本指明包含X11库的目录的方法。


--with-krb-srvnam=NAME
Kerberos服务主的名称． 缺省是 “postgres”．通常没有理由改变这个值． 

--with-openssl=DIRECTORY
制作支持 SSL （加密的）联接的postgres． 这个选项需要安装 OpenSSL 包． DIRECTORY 参数声明 OpenSSL 安装的根目录；缺省时 /usr/local/ssl． 

configure 将在安装之前检查所需要的头文件和库文件以确信你的 OpenSSL 安装是充分的． 

--with-java
制作 JDBC 驱动以及相关的 Java 包． 这个选项要求你先安装 Ant （当然还要有 JDK）． 请参考程序员手册 里面 JDBC 驱动的文档获取更多信息． 

--enable-syslog
打开PostgreSQL 服务器使用 syslog 日志系统的功能． （使用这个功能并不意味着你必须用 syslog 做日志，也不是说 服务器缺省会做这些，而是给你一个在运行时使用这个选项目的可能．） 

--enable-debug
把 所有程序和库以带有调试符号的方式编译． 这意味着你可以通过一个调试器运行程序来分析问题． 这样做显著增大了最后安装的可执行文件的大小， 并且在非 gcc 的编译器上，这么做通常还要关闭编译器优化， 导致速度的下降．但是，如果有这些符号表的话，就可以极大 帮助定位可能发生问题的位置．目前，我们认为这个选项对于 生产用途而言是边际变量，但是如果你正在进行开发工作，或者正在使用 beta 版本， 那么你就应该打开它． 

--enable-cassert
打开在服务器中的 assertion 检查， 它会检查许多“不可能发生”的条件．它对于代码开发的用途 而言是无价之宝，不过这些测试稍微地减慢了一些速度． 这些断言检查并不一定都是针对严重错误的，因此一些相对无害的 小虫子也可能导致 postmaster 重启--只要它触发了一次断言失败． 目前，我们不推荐在生产环境中使用这个选项，但是如果你在做开发 或者在使用 beta 版本的时候应该打开它． 
```
