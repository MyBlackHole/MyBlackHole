# mounts

挂载记录
/proc/mounts

```shell
<!-- 挂载记录 -->
cat /proc/mounts
sysfs /sys sysfs rw,nosuid,nodev,noexec,relatime 0 0
proc /proc proc rw,nosuid,nodev,noexec,relatime 0 0
udev /dev devtmpfs rw,nosuid,relatime,size=7807208k,nr_inodes=1951802,mode=755,inode64 0 0
devpts /dev/pts devpts rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000 0 0
tmpfs /run tmpfs rw,nosuid,nodev,noexec,relatime,size=1569980k,mode=755,inode64 0 0
efivarfs /sys/firmware/efi/efivars efivarfs rw,nosuid,nodev,noexec,relatime 0 0
/dev/nvme0n1p2 / ext4 rw,relatime,errors=remount-ro 0 0
securityfs /sys/kernel/security securityfs rw,nosuid,nodev,noexec,relatime 0 0
tmpfs /dev/shm tmpfs rw,nosuid,nodev,inode64 0 0
tmpfs /run/lock tmpfs rw,nosuid,nodev,noexec,relatime,size=5120k,inode64 0 0
cgroup2 /sys/fs/cgroup cgroup2 rw,nosuid,nodev,noexec,relatime,nsdelegate,memory_recursiveprot 0 0
pstore /sys/fs/pstore pstore rw,nosuid,nodev,noexec,relatime 0 0
bpf /sys/fs/bpf bpf rw,nosuid,nodev,noexec,relatime,mode=700 0 0
systemd-1 /proc/sys/fs/binfmt_misc autofs rw,relatime,fd=29,pgrp=1,timeout=0,minproto=5,maxproto=5,direct,pipe_ino=32057 0 0
hugetlbfs /dev/hugepages hugetlbfs rw,relatime,pagesize=2M 0 0
mqueue /dev/mqueue mqueue rw,nosuid,nodev,noexec,relatime 0 0
debugfs /sys/kernel/debug debugfs rw,nosuid,nodev,noexec,relatime 0 0
tracefs /sys/kernel/tracing tracefs rw,nosuid,nodev,noexec,relatime 0 0
fusectl /sys/fs/fuse/connections fusectl rw,nosuid,nodev,noexec,relatime 0 0
configfs /sys/kernel/config configfs rw,nosuid,nodev,noexec,relatime 0 0
ramfs /run/credentials/systemd-sysusers.service ramfs ro,nosuid,nodev,noexec,relatime,mode=700 0 0
ramfs /run/credentials/systemd-sysctl.service ramfs ro,nosuid,nodev,noexec,relatime,mode=700 0 0
ramfs /run/credentials/systemd-tmpfiles-setup-dev.service ramfs ro,nosuid,nodev,noexec,relatime,mode=700 0 0
/dev/nvme0n1p1 /boot/efi vfat rw,relatime,fmask=0077,dmask=0077,codepage=437,iocharset=iso8859-1,shortname=mixed,errors=remount-ro 0 0
/dev/loop0 /snap/android-studio/125 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/loop1 /snap/android-studio/126 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/loop2 /snap/arduino/70 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/loop3 /snap/arduino/85 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/loop4 /snap/bare/5 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/loop5 /snap/core/14784 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/loop6 /snap/core/14946 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/loop7 /snap/core18/2745 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/loop8 /snap/core18/2751 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/loop9 /snap/core20/1879 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/loop10 /snap/core20/1891 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/loop12 /snap/core22/634 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/loop13 /snap/datagrip/170 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/loop14 /snap/datagrip/171 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/loop15 /snap/flutter/130 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/loop16 /snap/flutter/141 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/loop17 /snap/gnome-3-28-1804/194 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/loop18 /snap/gnome-3-28-1804/198 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/loop19 /snap/gnome-3-38-2004/137 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/loop20 /snap/gnome-3-38-2004/140 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/loop21 /snap/gnome-42-2204/102 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/loop22 /snap/gnome-42-2204/99 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/loop23 /snap/gtk-common-themes/1534 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/loop24 /snap/gtk-common-themes/1535 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/loop25 /snap/snap-store/638 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/loop26 /snap/snap-store/959 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/loop27 /snap/snapd-desktop-integration/83 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
ramfs /run/credentials/systemd-tmpfiles-setup.service ramfs ro,nosuid,nodev,noexec,relatime,mode=700 0 0
binfmt_misc /proc/sys/fs/binfmt_misc binfmt_misc rw,nosuid,nodev,noexec,relatime 0 0
tmpfs /run/user/1000 tmpfs rw,nosuid,nodev,relatime,size=1569976k,nr_inodes=392494,mode=700,uid=1000,gid=1000,inode64 0 0
gvfsd-fuse /run/user/1000/gvfs fuse.gvfsd-fuse rw,nosuid,nodev,relatime,user_id=1000,group_id=1000 0 0
portal /run/user/1000/doc fuse.portal rw,nosuid,nodev,relatime,user_id=1000,group_id=1000 0 0
tmpfs /run/snapd/ns tmpfs rw,nosuid,nodev,noexec,relatime,size=1569980k,mode=755,inode64 0 0
nsfs /run/snapd/ns/snapd-desktop-integration.mnt nsfs rw 0 0
/dev/loop28 /snap/core22/750 squashfs ro,nodev,relatime,errors=continue,threads=single 0 0
/dev/sda1 /media/black/Data ext4 rw,nosuid,nodev,relatime,errors=remount-ro 0 0
```
