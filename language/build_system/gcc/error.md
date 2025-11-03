# error

- 错误：只允许在 C99 模式下使用‘for’循环初始化声明
```shell
gcc xxx.c --std=c99
```

- 错误： ‘enum fsconfig_command’多次定义
https://github.com/gcc-mirror/gcc/commit/2701442d0cf6292f6624443c15813d6d1a3562fe

- sanitizer_common/sanitizer_internal_defs.h:254:72: error: size of array ‘assertion_failed__1141’ is negative
```shell
configure 加入
--disable-libsanitizer

```

- temporary of non-literal type ‘google::protobuf::Any’ in a constant expression
```shell
升级 gcc >= 7.4
```

- configure: error: C++ preprocessor "/lib/cpp" fails sanity check
```shell
yum install gcc-c++
yum install glibc-headers (不一定要)
```

- cc1plus没有找到或目录
```shell
yum install gcc-c++
```

- <optimized out>
```shell
-O0：这个等级（字母“O”后面跟个零）关闭所有优化选项，也是CFLAGS或CXXFLAGS中没有设置-O等级时的默认等级。这样就不会优化代码，这通常不是我们想要的。
-O1：这是最基本的优化等级。编译器会在不花费太多编译时间的同时试图生成更快更小的代码。这些优化是非常基础的，但一般这些任务肯定能顺利完成。
-O2：-O1的进阶。这是推荐的优化等级，除非你有特殊的需求。-O2会比-O1启用多一些标记。设置了-O2后，编译器会试图提高代码性能而不会增大体积和大量占用的编译时间。
-O3：这是最高最危险的优化等级。用这个选项会延长编译代码的时间，并且在使用gcc4.x的系统里不应全局启用。自从3.x版本以来gcc的行为已经有了极大地改变。在3.x，-O3生成的代码也只是比-O2快一点点而已，而gcc4.x中还未必更快。用-O3来编译所有的软件包将产生更大体积更耗内存的二进制文件，大大增加编译失败的机会或不可预知的程序行为（包括错误）。这样做将得不偿失，记住过犹不及。在gcc 4.x.中使用-O3是不推荐的。
-Os：这个等级用来优化代码尺寸。其中启用了-O2中不会增加磁盘空间占用的代码生成选项。这对于磁盘空间极其紧张或者CPU缓存较小的机器非常有用。但也可能产生些许问题，因此软件树中的大部分ebuild都过滤掉这个等级的优化。使用-Os是不推荐的。
正如前面所提到的，-O2是推荐的优化等级。如果编译软件出现错误，请先检查是否启用了-O3。再试试把CFLAGS和CXXFLAGS倒回到较低的等级，如-O1甚或-O0 -g2 -ggdb（用来报告错误和检查可能存在的问题），再重新编译。
-O0 不进行优化处理。
-O 或 -O1 优化生成代码。
-O2 进一步优化。
-O3 比 -O2 更进一步优化，包括 inline 函数。
```

- defined but not used [-Wunused-function] 
```shell
<!-- 使用 __attribute__((unused)) 告诉编译器忽略此告警 -->
__attribute__((unused))

__attribute__((unused)) static char const* xxxx()
{
}
```

- relocation R_X86_64_32 against `.xxx‘ can not be used when making a PIE object； recompile with -fPIE
```shell
<!--加入 -fPIC 选项, 重新编译-->
./configure CFLAGS='-g -O0 -fPIC' --enable-debug --prefix=/run/media/black/Data/Documents/github/c/aliyun-oss-c-sdk/build/install/apr/
```

- Wincompatible-pointer-types
```c
// 场景是为了支持多种参数类型

typedef struct __db_config_desc {
	char *name;		/* The name of a simple DB_CONFIG command. */
	__db_config_type type;	/* The enum describing its argument type(s). */
	int (*func)();		/* The function to call with the argument(s). */
} CFG_DESC;

// func 具有以下几个可能
typedef int (*CFG_FUNC_STRING) __P((DB_ENV *, const char *));
typedef int (*CFG_FUNC_INT) __P((DB_ENV *, int));
typedef int (*CFG_FUNC_LONG) __P((DB_ENV *, long));
typedef int (*CFG_FUNC_UINT) __P((DB_ENV *, u_int32_t));
typedef int (*CFG_FUNC_2INT) __P((DB_ENV *, int, int));
typedef int (*CFG_FUNC_2UINT) __P((DB_ENV *, u_int32_t, u_int32_t));

static const CFG_DESC config_descs[] = {
    { "add_data_dir",		CFG_DIR,	__env_add_data_dir	},
    { "db_data_dir",		CFG_DIR,	__env_set_data_dir	},
    { "db_log_dir",		CFG_DIR,	__log_set_lg_dir	},
    { "db_tmp_dir",		CFG_DIR,	__env_set_tmp_dir	},
    ......
};

// 在代码中局部禁用
#pragma GCC diagnostic push
#pragma GCC diagnostic ignored "-Wincompatible-pointer-types"
// 有问题的代码
int (*func)(void) = (int (*)(void))some_function;
#pragma GCC diagnostic pop

// 或者全局禁用（不推荐）
-Wno-incompatible-pointer-types

```
