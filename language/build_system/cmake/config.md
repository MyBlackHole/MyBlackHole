# config

- 指定 glibc
```shell
<!-- 不建议 -->
export LD_LIBRARY_PATH=/media/black/Data/lib/glibc/glibc-2.27_bin:$LD_LIBRARY_PATH

<!-- 或 -->

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -L/media/black/Data/lib/glibc/glibc-2.27_bin")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -L/media/black/Data/lib/glibc/glibc-2.27_bin")
```

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
```shell
# 开启 debug, 取消编译优化
# CMAKE_EXPORT_COMPILE_COMMANDS 开启 compile_commands.json 生成
cmake -DCMAKE_BUILD_TYPE=Debug \
      -DCMAKE_EXPORT_COMPILE_COMMANDS=1 \
      -DCMAKE_C_FLAGS_DEBUG="-O0 -g" \
      -DCMAKE_CXX_FLAGS_DEBUG="-O0 -g" ..
```


- 开启 compile_commands.json 生成
```shell
cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=1 ..

<!-- 或 -->

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
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

- 使用系统 lib m
```shell
target_link_libraries(oss_c_sdk_test m)
```

- 指定安装路径
```shell
 cmake -DCMAKE_INSTALL_PREFIX=/media/black/Data/lib/h2o/master -DCMAKE_EXPORT_COMPILE_COMMANDS=1 ..
```
