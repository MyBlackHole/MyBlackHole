# protobuf

数据序列化协议，Google开发的一种高效的结构化数据序列化格式。

- 24.x
```shell
git submodule update --init --recursive
<!-- BUILD_SHARED_LIBS: 构建动态 -->
<!-- protobuf_BUILD_TESTS: 跳过测试 -->
<!-- protobuf_SHARED_OR_STATIC -->
cmake -DCMAKE_INSTALL_PREFIX=/media/black/Data/lib/protobuf/main \
      -DCMAKE_EXPORT_COMPILE_COMMANDS=1 \
      -Dprotobuf_BUILD_TESTS=OFF \
      -DBUILD_SHARED_LIBS=ON \
      -Dprotobuf_SHARED_OR_STATIC=SHARED \
      -DCMAKE_C_FLAGS_DEBUG="-O0 -g" \
      -DCMAKE_CXX_FLAGS_DEBUG="-O0 -g" ..
```

- 3.19.x
```shell
./autogen.sh
./configure --prefix=/media/black/Data/lib/protobuf/3_19_x
make -j16
make install
```
