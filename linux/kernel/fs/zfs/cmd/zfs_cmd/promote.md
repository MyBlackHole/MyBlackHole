# promote

利用此功能可以克隆并替换文件系统，使源文件系统变为指定文件系统的克隆。
此外，通过此功能还可以销毁最初创建克隆所基于的文件系统。
如果没有克隆提升 (clone promotion) 功能
就无法销毁活动克隆的源文件系统

```shell
zfs create aiopool/v3
cd /aiopool/v3
cat /dev/urandom > 1.txt
zfs snapshot aiopool/v3@1
zfs clone aiopool/v3@1 aiopool/v3_clone

zfs list
NAME               USED  AVAIL     REFER  MOUNTPOINT
aiopool           2.44G  45.5G      104K  /aiopool
aiopool/v1        1.03G  46.5G       56K  -
aiopool/v2        1.03G  46.5G       56K  -
aiopool/v3         378M  45.5G      378M  /aiopool/v3
aiopool/v3_clone     0B  45.5G      378M  /aiopool/v3_clone

<!-- 提升克隆 -->
zfs promote aiopool/v3_clone (提升之后 aiopool/v3@1 变 aiopool/v3_clone@1)


zfs list
NAME               USED  AVAIL     REFER  MOUNTPOINT
aiopool           2.44G  45.5G      104K  /aiopool
aiopool/v1        1.03G  46.5G       56K  -
aiopool/v2        1.03G  46.5G       56K  -
aiopool/v3           0B  45.5G      378M  /aiopool/v3
aiopool/v3_clone   378M  45.5G      378M  /aiopool/v3_clone

zfs destroy -R aiopool/v3_clone@1


zfs list
NAME               USED  AVAIL     REFER  MOUNTPOINT
aiopool           2.44G  45.5G      104K  /aiopool
aiopool/v1        1.03G  46.5G       56K  -
aiopool/v2        1.03G  46.5G       56K  -
aiopool/v3_clone   378M  45.5G      378M  /aiopool/v3_clone
```
