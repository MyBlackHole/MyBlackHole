# demo

- check
```shell
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX=/media/black/Data/lib/check/master \
      -DCMAKE_EXPORT_COMPILE_COMMANDS=1 \
      -DCMAKE_C_FLAGS_DEBUG="-O0 -g" \
      -DCMAKE_CXX_FLAGS_DEBUG="-O0 -g"  \
      -DCMAKE_EXE_LINKER_FLAGS=-static ..

cmake \
      -DCMAKE_EXPORT_COMPILE_COMMANDS=1 \
      -DCMAKE_C_FLAGS_DEBUG="-O0 -g" \
      -DCMAKE_EXE_LINKER_FLAGS=-static ..
```
