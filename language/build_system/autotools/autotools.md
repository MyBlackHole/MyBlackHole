# autotools
GNU构建系统，是利用脚本和make程序在特定的平台上构建软件的过程。一般过程是configure，make，make install 三部曲。这种方式成为一种习惯，被广泛使用。  
为了简化可移植构建的难度，早期有一套AutoTools的工具帮助程序员构建软件。configure，make，make install三部曲，大多都是基于Auto Tools来构建的。Auto Tools是GNU程序的标准构建系统。


## 流程
1. 执行autoscan命令。这个命令主要用于扫描工作目录，并且生成configure.scan文件。
2. 修改configure.scan为configure.ac文件，并且修改配置内容。
3. 执行aclocal命令。扫描 configure.ac 文件生成 aclocal.m4文件。
4. 执行autoconf命令。这个命令将 configure.ac 文件中的宏展开，生成 configure 脚本。
5. 执行autoheader命令。该命令生成 config.h.in 文件。
6. 新增Makefile.am文件，修改配置内容
7. 执行automake --add-missing命令。该命令生成 Makefile.in 文件。
8. 执行 ./congigure命令。将Makefile.in命令生成Makefile文件。
9. 执行make命令。生成可执行文件。


* * *

## 不同视角的程序构建

### 用户视角

configure脚本是由软件开发者维护并发布的给用户使用的shell脚本。该脚本的作用是检测系统环境，最终目的是生成Make file和configure.h。  
make通过读取Make file文件，开始构建软件。  
make install可以将软件安装到默认或者指定的系统路径

> 作为用户，我们就是通过上述三步将软件安装到我们系统中的
> 
>   ![[imgs/Pasted image 20230425090604.png]]

+   在上图中，开发者在分发源码包时，除了源代码中的头文件(.h)和程序源文件(.c)，还有许多支持软件构建的文件和工具。
+   最重要的就是Makefile.in和config.h。
+   configure脚本执行时，将为每一个.in文件处理成对应的非.in文件，即生成：Makefile，src/Makefile，config.h 。大部分情况下，只有Makefile和config.h。
+   Makefile用于被make程序识别并构建软件，而config.h中定义的宏，有助于软件通过预编译来改变自身代码，来适应目标平台某些特殊性。  
    有些软件在configure阶段，还可以生成其他文件，这完全取决于软件本身。

##### configure

一般而言，configure主要检查当前目标平台的程序，库，头文件，函数等的兼容性。这些结果将作用于config.h和Makefile文件的生成，从而影响最终的编译。  
用户可以通过configure配置参数，来定制需要包含或者不需要包含的组件，安装路径等。大概可以分为五组：

+   安装路径相关
+   程序名配置
+   跨平台编译
+   动静态库选项
+   程序组件

configure在执行过程中，除了生成Makefile外，还会生成，但是不限于以下文件：

+   config.log日志文件
+   config.cache缓存文件。提高下一次configure的速度，-C指定
+   config.status实际调用编译工具构建软件的shell脚本

如果软件通过libtool构建，还会生成libtool脚本。

### 开发者视角

开发者除了编写软件本身的代码外，还需要负责生成构建软件所需要的文件和工具。因此对于开发者而言，要么自己编写构建用的脚本，要么选择部分依赖工具。Auto tools就是这样的工具。

Autotools包括了autoconf和automake等命令

##### autoreconf

autoreconf程序能够自动按照合理的顺序调用autoconf，automake，aclocal程序

  
![[imgs/Pasted image 20230425090632.png]]
##### configure.ac

configure.ac用于生成configure脚本，autoconf工具用来完成这一步。

例如下列的例子

```auto
AC_PREREQ
AC_PREREQ([2.63]) 
AC_INIT([st], [1.0], [zhoupingtkbjb@163.com]) 
AC_CONFIG_SRCDIR([src/main.c]) 
AC_CONFIG_HEADERS([src/config.h]) 
AM_INIT_AUTOMAKE([foreign]) 
 # Checks for programs. 
AC_PROG_CC 
AC_PROG_LIBTOOL 
 # Checks for libraries.
# Checks for header files. 
 # Checks for typedefs, structures, and compiler characteristics. 
 # Checks for library functions.  
AC_CONFIG_FILES([Makefile 
          src/Makefile 
          src/a/Makefile 
           src/b/Makefile]) 
AC_OUTPUT 

```

其中以AC\_开头的类似函数调用一样的代码，实际上时被称为“宏”的调用。  
这里的宏，与C语言中的宏概念类似，会被替换展开。  
configure.ac文件的一般布局是：

+   AC\_INIT
+   测试程序
+   测试函数库
+   测试头文件
+   测试类型定义
+   测试结构
+   测试编译器特性
+   测试库函数
+   测试系统调用
+   AC\_OUTPUT

**configure.ac标签说明**

| 标签 | 说明 |
| --- | --- |
| AC\_PREREQ | 声明autoconf要求的版本号 |
| AC\_INIT | 定义软件名称，版本号，联系方式 |
| AM\_INIT\_AUTOMAKE | 必须要的，参数为软件名和版本号 |
| AC\_CONFIG\_SCRDIR | 该宏用来侦测所指定的源码文件是否存在，来确定源码有效性。 |
| AC\_CONFIG\_HEADER | 该宏用来生成config.h文件，以便autoheader命令使用 |
| AC\_PROG\_CC | 指定编译器，默认GCC |
| AC\_CONFIG\_FILE | 生成相应的Makefile文件，不同目录下通过空格分隔 |
| AC\_OUTPUT | 用来设定configure所要产生的文件，如果是makefile，config会把它检查出来的结果带入makefile.in文件，产生合适的makefile |

m4是一个经典的宏工具。autoconf正是构建在m4之上，可以理解为autoconf预先定义了大量的，用户检查系统可移植性的宏，这些宏在展开就是大量的shell脚本。

所以编写configure.ac就需要对这些宏掌握熟练，并且合理调用。

##### autoscan和configure.scan

可以通过调用autoscan命令，得到一个初始化的configure.scan文件。然后重命名为configure.ac后，在此基础上编辑configure.ac。  
autoscan会扫描源码，并生成一些通用的宏调用，输入的声明，以及输出的声明。尽管autoscan十分方便，但是没人能够在构建之前，就把源码完全写好。  
因此,autoscan通常用于初始化configure.ac，即生成configure.ac的雏形文件configure.scan

##### autoheader和configure.h

autoheader命令扫描configure.ac文件，并确定如何生成config.h.in。每当configure.ac变化时，都可以通过执行autoheader更新config.h.in。  
在configure.ac通过AC\_CONFIG\_HEADERS(\[config.h\])告诉autoheader应当生成config.h.in的路径  
config.h包含了大量的宏定义，其中包括软件包的名字等信息，程序可以直接使用这些宏。更重要的是，程序可以根据其中的对目标平台的可移植相关的宏，通过条件编译，动态的调整编译行为。

##### automake和Makefil.am

手工编写Makefile是一件相当繁琐的事情，并且随着项目的复杂程序变大，编写难度越来越大。automake工具应运而生。  
可以编辑Makefile.am文件，并依靠automake来生成Makefile.in

##### aclocal

configure.ac实际是依靠宏展开来得到configure。因此，能否成功生成，取决于宏定义是否能够找到。  
autoconf会从自身安装路径下寻找事先定义好的宏。然而对于像automake，libtool，gettex等第三方扩展宏，autoconf便无从知晓。  
因此，aclocal将在configure.ac同一个目录下生成aclocal.m4，在扫描configure.ac过程中，将第三方扩展和开发者自己编写的宏定义复制进去。  
如此一来，autoconf遇到不认识的宏时，就会从aclocal.m4中查找
![[imgs/Pasted image 20230425090655.png]]
##### libtool

libtool试图解决不同平台下，库文件的差异。libtool实际是一个shell脚本，实际工作中，调用了目标平台的cc编译器和链接器，以及给予合适的命令行参数。  
libtool可以单独使用，也可以跟autotools集成使用。

##### 辅助文件

clocal.m4 该宏定义文件包含了第三方宏定义，用于autoconf展开configure.ac  
NEWS，README，AUTHORS，ChangeLog GNU软件标配  
config.guess，config.sub 由automake产生，两个用于目标平台检查的脚本  
depcomp install-sh 由automake产生，用于完成编译和安装的脚本  
missing 由automake产生  
ltmain.sh 由libtoolize产生，用于在configure阶段，配置生成可运行于目标平台的libtool脚本  
ylwrap 由automake产生  
autogen.sh 早期autoreconf并不存在，软件开发者就自己编写脚本，按照顺序调用autoconf，autoheader，automake等工具。这个文件就是这样的脚本。

### makefile文件生成的基本流程
![[imgs/Pasted image 20230903003954.png]]
![[imgs/Pasted image 20230425090717.png]]

## 基本命令具体介绍

### autoscan 命令

该命令主要用来生成初步configure.in  
该命令参数

```auto
 -h, --help
              print this help, then exit

       -V, --version
              print version number, then exit

       -v, --verbose
              verbosely report processing

       -d, --debug
              don't remove temporary files

   Library directories:
       -B, --prepend-include=DIR
              prepend directory DIR to search path

       -I, --include=DIR
              append directory DIR to search path
```

通过autoscan 生成的文件是autoscan.log configure.scan  
这里我们只编译一个main.c,我们看看configure.scan 文件

```auto
#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.69])
AC_INIT([FULL-PACKAGE-NAME], [VERSION], [BUG-REPORT-ADDRESS])
AC_CONFIG_SRCDIR([main.c])
AC_CONFIG_HEADERS([config.h])

# Checks for programs.
AC_PROG_CC

# Checks for libraries.

# Checks for header files.

# Checks for typedefs, structures, and compiler characteristics.

# Checks for library functions.

AC_OUTPUT
```

##### AC\_INIT

```auto
Macro: AC_INIT (package, version, [bug-report], [tarname], [url])
```

这个宏定义至少要有两个参数 package ，和version  
bug-report ，tarname，url是可选参数

```auto
AC_PACKAGE_NAME, PACKAGE_NAME
      Exactly package. 
AC_PACKAGE_TARNAME, PACKAGE_TARNAME
    Exactly tarname, possibly generated from package. 
AC_PACKAGE_VERSION, PACKAGE_VERSION
    Exactly version. 
AC_PACKAGE_STRING, PACKAGE_STRING
    Exactly ‘package version’. 
AC_PACKAGE_BUGREPORT, PACKAGE_BUGREPORT
    Exactly bug-report, if one was provided. Typically an email address, or URL to a bug management web page. 
AC_PACKAGE_URL, PACKAGE_URL
  Exactly url, if one was provided. If url was empty, but package begins with ‘GNU ’, then this defaults to ‘http://www.gnu.org/software/tarname/’, otherwise, no URL is assumed.
```

##### AC\_PREREQ

配置版本  
这个宏用于宏AC\_INIT 之前

> 举例  
> AC\_PREREQ(\[2.69\])

##### AC\_COPYRIGHT 和 AC\_REVISION

该两个宏定义是可选的，用来管理配置脚本的版本号

```auto
For example, this line in configure.ac:
          AC_REVISION([$Revision: 1.30 $])
produces this in configure:
          #!/bin/sh
          # From configure.ac Revision: 1.30
```

##### AC\_CONFIG\_SRCDIR

```auto
Macro: AC_CONFIG_SRCDIR (unique-file-in-source-dir)
```

配置文件入口

###### AC\_OUTPUT

该宏定义没有参数在.ac文件的末尾。

##### AM\_INIT\_AUTOMAKE(\[OPTIONS\])

Runs many macros required for proper operation of the generated Makefiles.

Today, `AM_INIT_AUTOMAKE` is called with a single argument: a space-separated list of Automake options that should be applied to every <samp>Makefile.am</samp> in the tree. The effect is as if each option were listed in `AUTOMAKE_OPTIONS` (see [Options](https://www.gnu.org/software/automake/manual/html_node/Options.html#Options)).

This macro can also be called in another, *deprecated* form: `AM_INIT_AUTOMAKE(PACKAGE, VERSION, [NO-DEFINE])`. In this form, there are two required arguments: the package and the version number. This usage is mostly obsolete because the <var style="font-style: italic; font-weight: inherit;">package</var> and <var style="font-style: italic; font-weight: inherit;">version</var> can be obtained from Autoconf’s `AC_INIT` macro. However, differently from what happens for `AC_INIT` invocations, this `AM_INIT_AUTOMAKE` invocation supports shell variables’ expansions in the `PACKAGE` and `VERSION` arguments (which otherwise defaults, respectively, to the `PACKAGE_TARNAME` and `PACKAGE_VERSION` defined via the `AC_INIT` invocation; see [The `AC_INIT` macro](http://www.gnu.org/software/autoconf/manual/html_node/AC_005fINIT.html#AC_005fINIT) in <cite style="font-style: inherit; font-weight: inherit;">The Autoconf Manual</cite>); and this can be still be useful in some selected situations. Our hope is that future Autoconf versions will improve their support for package versions defined dynamically at configure runtime; when (and if) this happens, support for the two-args `AM_INIT_AUTOMAKE` invocation will likely be removed from Automake.

```auto
If your configure.ac has:

AC_INIT([src/foo.c])
AM_INIT_AUTOMAKE([mumble], [1.5])
you should modernize it as follows:

AC_INIT([mumble], [1.5])
AC_CONFIG_SRCDIR([src/foo.c])
AM_INIT_AUTOMAKE
```

* * *

## 文件生成

我们从开发者角度的图例来看，我们需要的文件有configure.ac ，Makefile.am ，aclocal.m4  
而作为用户，我们需要的文件是 makefile.in config.h.in 文件

因此这里我们看看每个文件是如何生成的

+   configure.ac
+   Makefile.am
+   aclocal.m4
+   makefile.in
+   config.h.in

### 准备工作

我们暂时就准备一个main.c printC.h printC.c 文件  
main.c 的内容是

```auto
#include <stdio.h>
#include "printC.h"
int main(int argc, const char * argv[]) {
    fputs("nihao\n", stderr);
    printCFuncition();
    return 0;
}
```

同目录下，还有printC.h 和printC.m 文件，printC.h

```auto
#ifndef printC_h
#define printC_h

#include <stdio.h>
void printCFuncition(void);

#endif /* printC_h */

```

printC.c

```auto

#include "printC.h"


void printCFuncition(void){
    fputs("printC.c/n", stderr);
}

```

打开终端进入到该目录下

```auto
$ls
main.c printC.h printC.c
```

### configure.ac

该文件生成是通过 autoscan 命令生成的 ，具体如下  
在中断命令输入autoscan 命令

```auto
$ autoscan && ls
autoscan.log    configure.scan  main.c      printC.c    printC.h
```

然后重新命名 configure.scan 文件

```auto
$ mv configure.scan configure.ac && ls
autoscan.log    configure.ac    main.c      printC.c    printC.h
```

我们获取到了configure.ac 文件了。

> 这里我们还是需要修改下configure.ac 内容，为以后生成makefile文件做准备  
> 我们在AC\_INIT 下面添加macro AM\_INIT\_AUTOMAKE。  
> 在 AC\_CONFIG\_HEADERS 行下面添加mocro AC\_CONFIG\_FILES(\[Makefile\])  
> 见下图
> 
>   

### Makefile.am

该文件是我们手动需要创建的文件  
文件内容是

```auto
AUTOMAKE_OPTIONS=foreign
bin_PROGRAMS=maket
maket_SOURCES=main.c  printC.h printC.c
```

> maket 就是未来生成的程序名称  
> maket\_SOURCES 是该名称所使用的源文件

> 假设我们工程里面有很多文件，那么我们手动写文件很容易遗漏，因此，建议最好自己写个shell脚本自动生成该文件。不是很复杂

### aclocal.m4

aclocal.m4 文件是我们通过aclocal 命令生成的

```auto
$ aclocal && ls
Makefile.am autom4te.cache  configure.ac
aclocal.m4  autoscan.log    main.c printC.h printC.m
```

到这里，我们开发者需要的文件都已经生成。我们可以通过命令生成 configure ,config.h.in Makefile.in 文件了,这些文件是用户需要的文件。生成过程如下

### config.h.in

config.h.in 文件是通过autoheader 命令生成的

```auto
$ autoheader && ls
Makefile.am autom4te.cache  config.h.in main.c      printC.h
aclocal.m4  autoscan.log    configure.ac    printC.c
```

### makefile.in

makefile.in 文件是通过automake命令生成的

```auto
$ automake --add-missing && ls

Makefile.am autom4te.cache  config.h.in install-sh  printC.c
Makefile.in autoscan.log    configure.ac    main.c      printC.h
aclocal.m4  compile     depcomp     missing
```

这里我们发现生成了makefile.in 文件了

### configure

这里我们通过autoconf 命令生成 configure文件

```auto
autoconf && ls
Makefile.am autom4te.cache  config.h.in depcomp     missing
Makefile.in autoscan.log    configure   install-sh  printC.c
aclocal.m4  compile     configure.ac    main.c      printC.h

```

### makefile

makefile 文件是通过 configure 来生成的

```auto
$ ./configure && ls
Makefile    autom4te.cache  config.h.in configure.ac    missing
Makefile.am autoscan.log    config.log  depcomp     printC.c
Makefile.in compile     config.status   install-sh  printC.h
aclocal.m4  config.h    configure   main.c      stamp-h1
```

### make

使用make 命令生成最终的包

```auto
$make && ls
Makefile    autoscan.log    config.status   main.c      printC.h
Makefile.am compile     configure   main.o      printC.o
Makefile.in config.h    configure.ac    maket       stamp-h1
aclocal.m4  config.h.in depcomp     missing
autom4te.cache  config.log  install-sh  printC.c
```

### 运行 maket 包

```auto
$./maket
nihao
printC.c
```

## 文件依赖和命令之间的依赖关系

完全没有依赖的文件是

> Makefile.am 该文件是自己手动生成的  
> configure.ac 该文件是通过autoscan 命令生成

aclocal 命令依赖文件

> configure.ac 通过该命令和依赖文件我们可以生成aclocal.m4 文件。该文件是autoconf autoheader 和automake命令依赖文件，因此，执行上述三个命令前要执行aclocal文件生成aclocal.m4 文件

autoconf命令依赖文件

> configure.ac  
> aclocal.m4  
> 有两个文件我们执行autoconf 命令，生成configure文件

autoheader 命令依赖文件

> configure.ac  
> aclocal.m4  
> 这两个文件通过autoheader 命令生成config.h.in 文件

automake 命令依赖文件

> Makefile.am  
> 通过该命令我们能生成 Makefile.in文件

./configure 脚本依赖文件

> Makefile.in  
> config.h.in  
> 通过该命令我们就能生产所需要的Makefile文件了

make 命令依赖文件

> makefile文件  
> 通过 make命名使用makefile文件，生成最终的可执行文件

## automake 支持的文件结构

automake支持三种文件夹层次：flat、shallow和deep。

+   flat(平)，指的是全部文件都位于同一个文件夹中  
    就是全部源文件、头文件以及其它库文件都位于当前文件夹中，且没有子文件夹。上述实例就是这样的
    
+   shallow(浅)，指的是基本的源码都储存在顶层文件夹，其它各个部分则储存在子文件夹中  
    就是主要源文件在当前文件夹中，而其他一些实现各部分功能的源文件位于各自不同的文件夹。automake本身就是这一类。
    
+   deep(深)，指的是全部源码都被储存在子文件夹中；顶层文件夹主要包括配置信息
    

不管对于上述那种方式，最简单的方式如下

假设目录结构如下

  

image.png

这里我们只需要修改下Makeflile.am文件就可以了

以前的makefile.am 文件

```auto
AUTOMAKE_OPTIONS=foreign
bin_PROGRAMS=maket
maket_SOURCES=main.c  printC.h printC.c
```

修改成如下就可以了

```auto
AUTOMAKE_OPTIONS=foreign
bin_PROGRAMS=maket
maket_SOURCES=main.c  printC.h printC.c deepfile/deepPrint.h deepfile/deepPrint.c

```

其他操作命令不变。

