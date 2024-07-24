# vcpkg


```shell
export VCPKG_ROOT=$HOME/.local/share/vcpkg
git clone https://github.com/microsoft/vcpkg $VCPKG_ROOT

-DCMAKE_TOOLCHAIN_FILE=${VCPKG_ROOT}/scripts/buildsystems/vcpkg.cmake

vcpkg install gtest

lvim CMakeLists.txt
find_package(GTest CONFIG REQUIRED)

rm build
mkdir build
<!-- 重新构建 -->
```
