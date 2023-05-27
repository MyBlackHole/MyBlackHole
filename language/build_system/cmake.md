# cmake

## build
```shell
./configure --prefix=/media/black/Data/lib/cmake/cmake_3_16_0   ##prefix为编译结果路径
make –j8
make install
```

## 例子
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
