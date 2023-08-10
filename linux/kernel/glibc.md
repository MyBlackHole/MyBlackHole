# glibc

```shell
sudo apt install gawk

mkdir build

cd build

<!-- ../configure CFLAGS='-g -O0' --prefix=/media/black/Data/lib/glibc/master -->
<!-- 不能不优化编译 -->
../configure CFLAGS='-g -O2' --prefix=/media/black/Data/lib/glibc/master

make -j16

sudo make install

sudo ln -s /use/local/glibc-xxx/lib /lib32或64
```
