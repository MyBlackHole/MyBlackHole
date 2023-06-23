# glibc

```shell
mkdir build

cd build

../configur —prefix=/use/local/glibc

make -j16

sudo make install

sudo ln -s /use/local/glibc-xxx/lib /lib32或64
```