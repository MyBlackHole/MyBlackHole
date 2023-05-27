# glibc

## build
- 异常
```shell
git clone https://sourceware.org/git/glibc.git
cd glibc
mkdir build
cd build
../configure CFLAGS="-Werror=use-after-free -g -O2" --prefix=/media/black/Data/lib/glibc/glibc-2.33
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
