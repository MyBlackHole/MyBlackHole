# 


## 例子
- 生成 compile_commands.json
```shell
cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=1
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
