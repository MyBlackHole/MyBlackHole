-  undefined reference to `pthread_key_create@GLIBC_2.34'
```shell
SET（CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -lpthread")
```

- 静态库配置
```shell
./config no-asm no-shared ....
```

- unrecognized command-line option ‘-Wdeprecated-non-prototype’
```shell

```

- ERROR: Problem encountered: missing python module: elftools
```shell
sudo apt-get install python3-pyelftools
```

- version `GLIBCXX_3.4.30' not found (required by /lib/x86_64-linux-gnu/libicuuc.so.72)
```shell
strings /usr/lib/x86_64-linux-gnu/libstdc++.so.6 | grep GLIBC

ln -sf /usr/lib/x86_64-linux-gnu/libstdc++.so.6 /media/black/Data/lib/gcc/gcc-releases-gcc-7.3.0_src/x86_64-pc-linux-gnu/libstdc++-v3/src/.libs/libstdc++.so.6
```

- not proc/sysinfo.h
```shell
wget http://ports.ubuntu.com/pool/main/p/procps/libprocps-dev_3.3.16-1ubuntu2.3_arm64.deb
```

- error: #error "glibc cannot be compiled without optimization"
```shell
CFLAGS="-g -O2"
```

- error: ‘memmove’ specified bound between 9223372036854775808 and 18446744073709551615 exceeds maximum object size 9223372036854775807 [-Werror=stringop-overflow=]
```shell
vfprintf-internal.c:2093:15: error: 'memmove' specified bound between 9223372036854775808 and 18446744073709551615 exceeds maximum object size 9223372036854775807 [-Werror=stringop-overflow=]
 2093 |               memmove (w, s, (front_ptr -s) * sizeof (CHAR_T));
      |               ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


--- a/stdio-common/vfprintf-internal.c
+++ b/stdio-common/vfprintf-internal.c
@@ -2090,7 +2090,8 @@ group_number (CHAR_T *front_ptr, CHAR_T *w, CHAR_T *rear_ptr,
 	    copy_rest:
 	      /* No further grouping to be done.  Copy the rest of the
 		 number.  */
-	      memmove (w, s, (front_ptr -s) * sizeof (CHAR_T));
+	      w -= s - front_ptr;
+	      memmove (w, front_ptr, (s - front_ptr) * sizeof (CHAR_T));
 	      break;
 	    }
 	  else if (*grouping != '\0')

-- 
```

- pointer may be used after 'realloc' [-Werror=use-after-free]
```shell
In function 'xrealloc',
    inlined from 'add_cmdname' at help.c:24:2: subcmd-util.h:56:23: error: pointer may be used after 'realloc' [-Werror=use-after-free]
   56 |                 ret = realloc(ptr, size);
      |                       ^~~~~~~~~~~~~~~~~~
subcmd-util.h:52:21: note: call to 'realloc' here
   52 |         void *ret = realloc(ptr, size);
      |                     ^~~~~~~~~~~~~~~~~~
subcmd-util.h:58:31: error: pointer may be used after 'realloc' [-Werror=use-after-free]
   58 |                         ret = realloc(ptr, 1);
      |                               ^~~~~~~~~~~~~~~
subcmd-util.h:52:21: note: call to 'realloc' here
   52 |         void *ret = realloc(ptr, size);
      |                     ^~~~~~~~~~~~~~~~~~

--- a/tools/lib/subcmd/subcmd-util.h
+++ b/tools/lib/subcmd/subcmd-util.h
@@ -50,15 +50,8 @@ static NORETURN inline void die(const ch
 static inline void *xrealloc(void *ptr, size_t size)
 {
 	void *ret = realloc(ptr, size);
-	if (!ret && !size)
-		ret = realloc(ptr, 1);
-	if (!ret) {
-		ret = realloc(ptr, size);
-		if (!ret && !size)
-			ret = realloc(ptr, 1);
-		if (!ret)
-			die("Out of memory, realloc failed");
-	}
+	if (!ret)
+		die("Out of memory, realloc failed");
 	return ret;
 }
```

- 
ERROR: ld.so: object 'libgtk3-nocsd.so.0' from LD_PRELOAD cannot be preloaded (cannot open shared object file): ignored.
error: 'daemo' is not a recognised command
       Did you mean daemon?
Try 'nix --help' for more information.
```
sudo apt-get install libgtk3-nocsd0:i386
sudo apt-get install libgtk3-nocsd0


export LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libgtk3-nocsd.so.0


sudo apt install gtk3-nocsd


```
