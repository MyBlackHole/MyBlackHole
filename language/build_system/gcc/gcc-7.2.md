# build 7.2

```shell
g++：yum install gcc-c++
lbzip2：yum -y install lbzip2 bz2
yum -y install texinfo
```


```shell
# download gcc-7.4.0 source code
wget https://mirrors.tuna.tsinghua.edu.cn/gnu/gcc/gcc-7.4.0/gcc-7.4.0.tar.gz --no-check-certificate

# extract the source code
tar -xvf gcc-7.4.0.tar.gz

# fetch dependency
cd gcc-7.4.0
./contrib/download_prerequisites

# If failed to download the dependency due to network failure, 
# please manually download the dependency and move them to the root path of gcc, 
# the version of dependency follow the document contrib/download_prerequisites
# The download link is https://gcc.gnu.org/pub/gcc/infrastructure/
# mv mpfr-3.1.4 gcc-7.4.0/mpfr

# create the target installation directory
mkdir build

# generate configuration with target installation path( please use absolutely path). 
./configure --prefix=/path/to/gcc-7.4.0/build -disable-multilib

# build
make -j 8
make install

# setup the env variables
export PATH=/path/to/gcc-7.4.0/build/bin:$PATH
export CC=`which gcc`
export CXX=`which g++`
export LD_LIBRARY_PATH=/path/to/gcc-7.4.0/build/lib64:$LD_LIBRARY_PATH

# update gcc shared library
sudo cp /path/to/gcc-7.4.0/build/lib64/libstdc++.so.6.0.24 /lib64
sudo rm /lib64/libstdc++.so.6
sudo ln -s /lib64/libstdc++.so.6.0.24 /lib64/libstdc++.so.6

# check gcc shared library
ll /lib64/libstdc++.so.6

# check version，here show 7.4.0
gcc --version
```
