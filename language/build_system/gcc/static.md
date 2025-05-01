# static

将所有 .o 文件打包成一个.a 文件，不会进行链接。

```shell
-static 代表使用静态链接库，-L.代表静态链接库搜索路径 .代表当前路径
```

```shell
使用-Wl,-Bstatic，-Wl,-Bdynamic选项，将部分动态库设置为静态链接。
gcc使用-Wl将参数传递给连接器。链接器使用-Bdynamic强制连接动态库，-Bstatic强制连接静态库。所以部分静态，部分动态连接这么写：
gcc -Wl,-Bstatic -l<static-lib> -Wl,-Bdynamic -l<dynamic-lib>

```

- PIC
```shell
-fPIC 产生位置无关代码，编译出来的代码可以被不同地址空间的程序加载运行。
```
