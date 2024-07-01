# make

## 参数
-C: 改变目录
-d: 打印调试信息
-f: 选定其他文件当Makefile
-B: 强制编译所有的目标文件

## install
```
apt-get install make
```

- 显示编译命令
```shell
# make 追加
make VERBOSE=1
```

- 清空所有
```shell
make distclean
```

- 静态编译
[binutils](http://ftp.gnu.org/gnu/binutils/)
```shell
<!-- binutils -->
<!-- libtool 支持 -->
make LDFLAGS=-all-static
```

- 通用静态编译
```shell
./configure CFLAGS=-static --enable-static LDFLAGS=-static --disable-shared
或
./configure CFLAGS=-static LDFLAGS=-static
或
make CFLAGS=-static LDFLAGS=-static
```

```shell
--Werror
视警告为错误;出现任何警告即放弃编译.

-Wall 
会打开一些很有用的警告选项，建议编译时加此选项。

-Wextra 
打印一些额外的警告信息。

-w 
禁止显示所有警告信息。
```
