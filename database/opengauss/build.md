# build

- 组件构建
```shell
# podman run \
#     -v $MYHOME:$MYHOME \
#     --rm -it dokken/centos-7 bash

# yum install bison flex readline-devel libaio-devel glibc-devel ncurses-devel patch lsb_release

export MYHOME=/run/media/black/Data
export CODE_BASE=$MYHOME/Documents/github/Cpp/openGauss-server     # openGauss-server的路径(不支持软链?)
export BINARYLIBS=$MYHOME/Documents/github/demo/OpenGauss/openGauss-third_party_binarylibs_Centos7.6_x86_64    # binarylibs的路径
export GAUSSHOME=$CODE_BASE/dest/
export GCC_PATH=$BINARYLIBS/buildtools/gcc10.3    # gcc的版本，根据三方包中对应的gcc版本进行填写即可，一般有gcc7.3或gcc10.3两种
export CC=$GCC_PATH/gcc/bin/gcc
export CXX=$GCC_PATH/gcc/bin/g++
export LD_LIBRARY_PATH=$GAUSSHOME/lib:$GCC_PATH/gcc/lib64:$GCC_PATH/isl/lib:$GCC_PATH/mpc/lib/:$GCC_PATH/mpfr/lib/:$GCC_PATH/gmp/lib/:$LD_LIBRARY_PATH
export PATH=$GAUSSHOME/bin:$GCC_PATH/gcc/bin:$PATH

# gcc -V
# g++ -V

# ./configure --gcc-version=10.3.0 CC=g++ CFLAGS='-O0' --prefix=$GAUSSHOME --3rd=$BINARYLIBS --enable-debug --enable-cassert --enable-thread-safety --with-readline --without-zlib
cd ./contrib/pg_xlogdump/

# # bear 2.4.4 执行方式
# bear make

make
```

- 构建
```shell
export MYHOME=/run/media/black/Data
export CODE_BASE=$MYHOME/Documents/github/Cpp/openGauss-server     # openGauss-server的路径(不支持软链?)
export BINARYLIBS=$MYHOME/Documents/github/demo/OpenGauss/openGauss-third_party_binarylibs_Centos7.6_x86_64    # binarylibs的路径
export GAUSSHOME=$CODE_BASE/dest/
export GCC_PATH=$BINARYLIBS/buildtools/gcc10.3    # gcc的版本，根据三方包中对应的gcc版本进行填写即可，一般有gcc7.3或gcc10.3两种
export CC=$GCC_PATH/gcc/bin/gcc
export CXX=$GCC_PATH/gcc/bin/g++
export LD_LIBRARY_PATH=$GAUSSHOME/lib:$GCC_PATH/gcc/lib64:$GCC_PATH/isl/lib:$GCC_PATH/mpc/lib/:$GCC_PATH/mpfr/lib/:$GCC_PATH/gmp/lib/:$LD_LIBRARY_PATH
export PATH=$GAUSSHOME/bin:$GCC_PATH/gcc/bin:$PATH

[user@linux openGauss-server]$ sh build.sh -m [debug | release | memcheck] -3rd [binarylibs path]

[user@linux openGauss-server]$ sh build.sh       # 编译安装release版本的openGauss。需代码目录下有binarylibs或者其软链接，否则将会失败。
[user@linux openGauss-server]$ sh build.sh -m debug -3rd /sda/binarylibs    # 编译安装debug版本的openGauss
[user@linux openGauss-server]$ sh build.sh -m debug -3rd /sda/binarylibs --cmake    # 使用cmake方式编译代码，不加--cmake参数，默认使用make方式

# gcc10.3.1版本（一般用于openEuler + ARM架构）
./configure --gcc-version=10.3.1 CC=g++ CFLAGS='-O0' --prefix=$GAUSSHOME --3rd=$BINARYLIBS --enable-debug --enable-cassert --enable-thread-safety --with-readline --without-zlib

# gcc10.3.0版本
./configure --gcc-version=10.3.0 CC=g++ CFLAGS='-O0' --prefix=$GAUSSHOME --3rd=$BINARYLIBS --enable-debug --enable-cassert --enable-thread-safety --with-readline --without-zlib


[user@linux openGauss-server]$ make -sj
[user@linux openGauss-server]$ make install -sj

---------------

sudo ln -s /lib/libtinfo.so.6 /lib/libtinfo.so.5
sh build.sh -m debug -3rd /run/media/black/Data/Documents/github/demo/OpenGauss/openGauss-third_party_binarylibs_Centos7.6_x86_64/
```
