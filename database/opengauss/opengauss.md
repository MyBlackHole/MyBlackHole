# opengauss

## build
```shell
CODE_BASE=$MYHOME/Documents/github/Cpp/openGauss-server-5.1.0     # openGauss-server的路径(不支持软链?)
BINARYLIBS=$MYHOME/Documents/github/Demo/OpenGauss/openGauss-third_party_binarylibs_Centos7.6_x86_64    # binarylibs的路径
GAUSSHOME=$CODE_BASE/dest
GCC_PATH=$BINARYLIBS/buildtools/gcc10.3    # gcc的版本，根据三方包中对应的gcc版本进行填写即可，一般有gcc7.3或gcc10.3两种
CC=$GCC_PATH/gcc/bin/gcc
CXX=$GCC_PATH/gcc/bin/g++
LD_LIBRARY_PATH=$GAUSSHOME/lib:$GCC_PATH/gcc/lib64:$GCC_PATH/isl/lib:$GCC_PATH/mpc/lib/:$GCC_PATH/mpfr/lib/:$GCC_PATH/gmp/lib/:$LD_LIBRARY_PATH
PATH=$GAUSSHOME/bin:$GCC_PATH/gcc/bin:$PATH

```

<!-- - archlinux 环境下编译 -->
<!-- ```shell -->
<!-- <!-- 缺少 libssl.so.1.1，需要安装 openssl-1.1 --> -->
<!-- parn -S openssl-1.1 -->

<!-- paru -S gcc10 -->
<!-- ``` -->
- podman 环境下编译
```shell
podman run \
    -v $MYHOME:$MYHOME \
    --rm -it dokken/centos-7 bash

<!--podman run \-->
<!--    -v $MYHOME:$MYHOME \-->
<!--    --name opengauss_dev \-->
<!--    -it localhost/opengauss:dev bash-->

yum install bison flex readline-devel libaio-devel glibc-devel ncurses-devel patch lsb_release perl-devel gcc gcc-c++ make cmake3 git wget unzip -y
```

## podman
```shell
podman pull enmotech/opengauss:latest

podman run --name opengauss --privileged=true -d -e GS_PASSWORD=openGauss@123 -p5432:5432 enmotech/opengauss:latest

podman exec -it opengauss bash

# 安装、数据目录
cd /usr/local/opengauss
cd /var/lib/opengauss

su - omm


# 连接测试
gsql -d postgres -U gaussdb -W 'openGauss@123' -h 127.0.0.1 -p 5432

[opengauss@d41ae4f5cf7b bin]$ gsql
gsql ((openGauss 6.0.0 build aee4abd5) compiled at 2024-09-29 19:14:27 commit 0 last mr  )
Non-SSL connection (SSL connection is recommended when requiring high-security)
Type "help" for help.

opengauss=# show data_directory;
     data_directory
-------------------------
 /var/lib/opengauss/data
(1 row)

```

## 命令
- 查询所有数据库
```shell
\l
```

- 查询数据目录
```shell
omm=# show data_directory;
     data_directory
-------------------------
 /var/lib/opengauss/data
(1 row)
```
