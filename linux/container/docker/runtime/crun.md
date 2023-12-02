# crun (推荐)
C 实现 OCI

## build
```shell
sudo apt-get install -y make git gcc build-essential pkgconf libtool \
   libsystemd-dev libprotobuf-c-dev libcap-dev libseccomp-dev libyajl-dev \
   libgcrypt20-dev go-md2man autoconf python3 automake
./autogen.sh
# ./configure CFLAGS='-I/usr/include/libseccomp'
./configure CFLAGS='-g -O0 -I/usr/include/libseccomp'
# --enable-shared 启用共享库构建
make
```

## 
```shell
# podman --runtime /usr/bin/crun run --rm --memory 4M fedora echo it works
it works
```
