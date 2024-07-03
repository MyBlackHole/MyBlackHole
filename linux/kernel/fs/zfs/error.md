- cannot set property for 'aiopool/test_write_full': 'sharenfs' does not apply to datasets of this type


-  Kernel built with CONFIG_DEBUG_LOCK_ALLOC which is incompatible
with the CDDL license and will prevent the module linking stage
from succeeding. You must rebuild your kernel without this
option enabled.
```shell
License 设置为 GPL 即可。

vi META
Meta:          1
Name:          zfs
Branch:        1.0
Version:       2.2.99
Release:       1
Release-Tags:  relext
License:       GPL
Author:        OpenZFS
Linux-Maximum: 6.8
Linux-Minimum: 3.10
```
