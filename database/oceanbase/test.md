# test

允许测试

```shell
<!-- 所有单元 -->
cd build_debug/unittest/
make -j 4
./run_tests.sh


<!-- 单个单元 -->
cd build_debug/unittest/sql/common
make -j 4
./test_base64_encode

```

- mysql 测试
```shell
<!-- 全面测试 -->
cd tools/deploy/mysql_test/
obd test mysqltest <deploy name> --all
```
