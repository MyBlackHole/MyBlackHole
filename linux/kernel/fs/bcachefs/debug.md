# debug

```shell
# fuse
BCACHEFS_FUSE=1 make clean
BCACHEFS_FUSE=1 bear -- make -j10
BCACHEFS_FUSE=1 make
BCACHEFS_FUSE=1 make install
ln -sf target/debug/bcachefs .

❯ ./bcachefs fusemount --help
Usage: ./bcachefs fusemount [options] <dev>[:dev2:...] <mountpoint>

    -h   --help            print help
    -V   --version         print version
    -d   -o debug          enable debug output (implies -f)
    -f                     foreground operation
    -s                     disable multi-threaded operation
    -o clone_fd            use separate fuse device fd for each thread
                           (may improve performance)
    -o max_idle_threads    the maximum number of idle worker threads
                           allowed (default: -1)
    -o max_threads         the maximum number of worker threads
                           allowed (default: 10)
    -o allow_other         allow access by all users
    -o allow_root          allow access by root
    -o auto_unmount        auto unmount on process termination

mkdir Debug
dd if=/dev/zero of=Debug/1G bs=1M count=1024

./bcachefs format Debug/1G
# mkfs.fuse.bcachefs Debug/1G
mkdir Debug/mount_1G
./bcachefs fusemount Debug/1G ./Debug/mount_1G/
Opening bcachefs filesystem on:
        Debug/1G
starting version 1.25: extent_flags
recovering from clean shutdown, journal seq 3
accounting_read... done
alloc_read... done
snapshots_read... done
going read-write
journal_replay... done
resume_logged_ops... done
delete_dead_inodes... done
Fuse mount initialized.
Fuse forcing to foreground mode, due gcc constructors usage.
fuse_init: activating writeback


./bcachefs fusemount -d -f -s -o auto_unmount -o allow_other Debug/1G ./Debug/mount_1G/
Opening bcachefs filesystem on:
        Debug/1G
starting version 1.25: extent_flags
recovering from clean shutdown, journal seq 4
accounting_read... done
alloc_read... done
snapshots_read... done
going read-write
journal_replay... done
resume_logged_ops... done
delete_dead_inodes... done
FUSE library version: 3.17.1
Fuse mount initialized.
unique: 2, opcode: INIT (26), nodeid: 0, insize: 104, pid: 0
INIT: 7.42
flags=0x73fffffb
max_readahead=0x00020000
fuse_init: activating writeback
   INIT: 7.40
   flags=0x40419029
   max_readahead=0x00020000
   max_write=0x00100000
   max_background=0
   congestion_threshold=0
   time_gran=1
   unique: 2, success, outsize: 80
```
