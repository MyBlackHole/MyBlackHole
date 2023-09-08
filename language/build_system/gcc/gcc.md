# gcc

## build
- 依赖
```shell
# 手动下载
gmp:http://ftp.gnu.org/gnu/gmp/gmp-6.1.1.tar.xz
mpfr:http://ftp.gnu.org/gnu/mpfr/mpfr-4.0.2.tar.gz
mpc:http://ftp.gnu.org/gnu/mpc/mpc-1.1.0.tar.gz
isl:https://gcc.gnu.org/pub/gcc/infrastructure/isl-0.18.tar.bz2

## gmp
./configure --prefix=/media/black/Data/lib/gmp/gmp_6_1_1
make –j8
make install

## mpfr
./configure --prefix=/media/black/Data/lib/mpfr/mpfr_4_0_2 --with-gmp=/media/black/Data/lib/gmp/gmp_6_1_1
make –j8
make install

## mpc
./configure --prefix=/media/black/Data/lib/mpc/mpc_1_1_0 --with-gmp=/media/black/Data/lib/gmp/gmp_6_1_1 --with-mpfr=/media/black/Data/lib/mpfr/mpfr_4_0_2
make –j8
make install

## isl
./configure --prefix=/media/black/Data/lib/isl/isl_4_0_2 --with-gmp-prefix=/media/black/Data/lib/gmp/gmp_6_1_1
make –j8
make install
-------
# 用其自带的下载脚本
./contrib/download_prerequisites
```
- gcc
```shell
sudo apt-get install bison build-essential flex


# 在 glibc>=2.28, 去掉了 ustat.h 文件，gcc源码需要删除相关信息
vim ./libsanitizer/sanitizer_common/sanitizer_platform_limits_posix.cc
## 注释掉如下内容
第157行 //#include <sys/ustat.h>
第250行 //unsigned struct_ustat_sz = sizeof(struct ustat);

# 导入环境变量
export LD_LIBRARY_PATH=/media/black/Data/lib/gmp/gmp_6_1_1/lib:/media/black/Data/lib/mpc/mpc_1_1_0/lib:/media/black/Data/lib/mpfr/mpfr_4_0_2/lib:/media/black/Data/lib/isl/isl_4_0_2/lib:/media/black/Data/lib/icu/icu_57_2/lib/:${LD_LIBRARY_PATH}

export C_INCLUDE_PATH=/media/black/Data/lib/gmp/gmp_6_1_1/include:/media/black/Data/lib/mpc/mpc_1_1_0/include:/media/black/Data/lib/mpfr/mpfr_4_0_2/include:/media/black/Data/lib/isl/isl_4_0_2sl/include:${C_INCLUDE_PATH}

# ./configure CFLAGS='-fstack-protector-strong -Wl,-z,noexecstack -Wl,-z,relro,-z,now' --prefix=/media/black/Data/lib/gcc/gcc_7_3_0 --with-gmp=/media/black/Data/lib/gmp/gmp_6_1_1 --with-mpfr=/media/black/Data/lib/mpfr/mpfr_4_0_2 --with-mpc=/media/black/Data/lib/mpc/mpc_1_1_0 --with-isl=/media/black/Data/lib/isl/isl_4_0_2 --disable-multilib --enable-languages=c,c++
./configure --prefix=/media/black/Data/lib/gcc/gcc_7_3_0 --with-gmp=/media/black/Data/lib/gmp/gmp_6_1_1 --with-mpfr=/media/black/Data/lib/mpfr/mpfr_4_0_2 --with-mpc=/media/black/Data/lib/mpc/mpc_1_1_0 --with-isl=/media/black/Data/lib/isl/isl_4_0_2 --disable-multilib --enable-languages=c,c++ --disable-libsanitizer
make -j8
make install
```


### 语法

-Wl,-rpath,/xxxx/lib (推荐)
-Wl,--dynamic-linker编译参数
设置LD_LIBRARY_PATH
设置PATH

- __builtin_expect
```
这个指令是gcc引入的，作用是允许程序员将最有可能执行的分支告诉编译器。这个指令的写法为：__builtin_expect(EXP, N)。
意思是：EXP==N的概率很大。

#define likely(x) __builtin_expect(!!(x), 1) //x很可能为真       
#define unlikely(x) __builtin_expect(!!(x), 0) //x很可能为假

if(likely(value))  //等价于 if(value)
if(unlikely(value))  //也等价于 if(value)

int x, y;
 if(unlikely(x > 0))
    y = 1; 
else 
    y = -1;
```
