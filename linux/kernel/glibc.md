# glibc

```shell
sudo apt install gawk

mkdir build

cd build

<!-- ../configure CFLAGS='-g -O0' --prefix=/media/black/Data/lib/glibc/master -->
<!-- 不能不优化编译 -->
export LD_LIBRARY_PATH=
../configure CFLAGS='-g -O2' --prefix=/media/black/Data/lib/glibc/master
<!-- ../configure  --disable-sanity-checks (直接覆盖系统的，危险操作!!!!!!!) -->

make -j16

sudo make install

sudo ln -s /use/local/glibc-xxx/lib /lib32或64
```

## build

- make 版本过低
```shell
<!-- 如果已经安装过 make -->
mv /usr/bin/make /usr/bin/make_old
```


- 异常
```shell
git clone https://sourceware.org/git/glibc.git
cd glibc
mkdir build
cd build
../configure CFLAGS="-Werror=use-after-free -g -O2" --prefix=/media/black/Data/lib/glibc/glibc-2.33
```

- libstdc++.so 版本未被更新
```shell
cp /usr/local/gcc-8.2.0/lib64/libstdc++.so.6.0.24  /usr/lib64/
<!-- cp /usr/local/lib64/libstdc++.so.6.0.24  /usr/lib64/ -->
```

- 查询版本支持
```shell
strings /usr/lib64/libc.so.6 |grep GLIBC_
```

- glibc-all-in-one
```shell
git clone https://github.com/matrix1001/glibc-all-in-one.git

cd glibc-all-in-one

python3 update_list

git clone https://github.com/NixOS/patchelf.git
cd patchelf
./bootstrap.sh 
./configure
make
make check
make install

./download 2.27-3ubuntu1_amd64

cd libs

```

# 临时放置
git clone git@github.com:unicode-org/icu.git

## 使用
```shell
# 然后通过环境变量来指定glibc

--dynamic-linker=/root/build/glibc-build/lib/ld-linux-x86-64.so.2
----
export LD_PRELOAD=/media/black/Data/lib/glibc/glibc-2.33/libc-2.33.so

export LD_LIBRARY_PATH=/media/black/Data/lib/glibc/glibc-2.33/lib
----

gcc -Wl,-rpath='/root/glibc-all-in-one/libs/2.27-3ubuntu1_amd64',-dynamic-linker='/root/glibc-all-in-one/libs/2.27-3ubuntu1_amd64/ld-linux-x86-64.so.2' -s 1.c -o haha



```

- 指定 glibc
```shell
./configure LDFLAGS="-static -L/media/black/Data/lib/glibc/master/lib/ -Wl,--rpath=/media/black/Data/lib/glibc/master/lib/ -Wl,--dynamic-linker=/media/black/Data/lib/glibc/master/lib/ld-linux-x86-64.so.2" \
    CFLAGS="-static -I /media/black/Data/lib/glibc/master/include/" --enable-static --disable-shared

CFLAGS=”-I/root/build/glibc-build/include” 这个是指定头文件的查找路径，去新编译的glibc里查找
LDFLAGS: Makefile的链接选项，其中
-L: 告诉链接器先从指定的路径查找库来链接，如果没找到，再从默认的地方找。编译时的-L选项并不影响环境变量LD_LIBRARY_PATH
-L 只是指定了程序编译连接时库的路径，并不影响程序执行时库的路径，系统还是会到默认路径下查找该程序所需要的库，如果找不到，还是会报错，类似cannot open shared object file。
可以设定LD_LIBRARY_PATH来让程序在运行时查找除默认路径（默认是先搜索/lib和/usr/lib这两个目录，然后按照/etc/ld.so.conf里面的配置搜索绝对路径）之外的其他路径，
不过LD_LIBRARY_PATH的设定作用是全局的，建议使用gcc的的-R或-rpath选项来在编译时就指定库的查找路径，并且该库的路径信息保存在可执行文件中，运行时它会直接到该路径查找库，避免了使用LD_LIBRARY_PATH环境变量查找。

因此我在上面指定了-Wl,–rpath=/root/build/glibc-build/lib，说明一下，-Wl,option是将选项传给链接器
```
