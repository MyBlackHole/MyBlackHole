# cmd 

参数

- 静态编译
```shell
<!-- 会去找静态库(没有就会报错) -->
-static
```

- 选择线程安全的实现
```shell
-pthread
<!-- 区别与 -lpthread(最大程度向下兼容) -->
```

