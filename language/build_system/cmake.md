# cmake

```shell
# 开启 debug, 取消编译优化
# CMAKE_EXPORT_COMPILE_COMMANDS 开启 compile_commands.json 生成
cmake -DCMAKE_BUILD_TYPE=Debug \
      -DCMAKE_EXPORT_COMPILE_COMMANDS=1 \
      -DCMAKE_C_FLAGS_DEBUG="-O0 -g" \
      -DCMAKE_CXX_FLAGS_DEBUG="-O0 -g" ..
```
