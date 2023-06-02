# cmake

## 配置
- 静态、动态
![[imgs/Pasted image 20230528115058.png]]

## build
```shell
./configure --prefix=/media/black/Data/lib/cmake/cmake_3_16_0   ##prefix为编译结果路径
make –j8
make install
```

## 例子
- 静态编译
```shell
-DCMAKE_EXE_LINKER_FLAGS=-static
或
set(CMAKE_EXE_LINKER_FLAGS "-static")
```

- 指定安装路径
```shell
-DCMAKE_INSTALL_PREFIX=/media/black/Data/lib/xtrabackup
或
##  PROJECT(<project_name>) 后加
SET(CMAKE_INSTALL_PREFIX "/media/black/Data/lib/xtrabackup" )
```

- 制定编译器
```shell
cmake .. -DCMAKE_CXX_COMPILER=/media/black/Data/lib/gcc/gcc_7_3_0/bin/g++ -DCMAKE_C_COMPILER=/media/black/Data/lib/gcc/gcc_7_3_0/bin/gcc
# 或
set (CMAKE_C_COMPILER "/usr/local/gcc/bin/gcc")
set (CMAKE_CXX_COMPILER "/usr/local/gcc/bin/g++")
```

```shell
# 开启 debug, 取消编译优化
# CMAKE_EXPORT_COMPILE_COMMANDS 开启 compile_commands.json 生成
cmake -DCMAKE_BUILD_TYPE=Debug \
      -DCMAKE_EXPORT_COMPILE_COMMANDS=1 \
      -DCMAKE_C_FLAGS_DEBUG="-O0 -g" \
      -DCMAKE_CXX_FLAGS_DEBUG="-O0 -g" ..
```

- 输出编译命令
```shell
# 1 cmake 追加
-DCMAKE_VERBOSE_MAKEFILE=ON

# 2 CMakeLists.txt 添加
set(CMAKE_VERBOSE_MAKEFILEON ON)

# 3 make 追加
VERBOSE=1
```

- 自定义头文件与连接库路径
```shell
include_directories(path/to/include/dir)

link_directories(path/to/library/dir)
```

- 生成 compile_commands.json
```shell
cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=1 ..
```

- debug 设置
```shell
mkdir Release  
cd Release  
cmake -DCMAKE_BUILD_TYPE=Release ..  

# 或

mkdir Debug
cd Debug
cmake -DCMAKE_BUILD_TYPE=Debug ..

# 或
# 修改 CMakeLists.txt
SET(CMAKE_BUILD_TYPE "Debug”)
or
SET(CMAKE_BUILD_TYPE "Release")
```
