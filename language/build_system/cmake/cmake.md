# cmake

## 配置
- 静态、动态
![[imgs/Pasted image 20230528115058.png]]

## build
```shell
./configure --prefix=/media/black/Data/lib/cmake/cmake_3_16_0   ##prefix为编译结果路径
make –j8
make install

<!-- centos -->
wget https://github.com/Kitware/CMake/releases/download/v3.22.1/cmake-3.22.1.tar.gz
tar -xzvf cmake-3.22.1.tar.gz && cd cmake-3.22.1/ && ./bootstrap && gmake && gmake install
```
