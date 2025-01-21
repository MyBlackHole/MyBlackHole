# files

查看进程打开的文件清单等

```shell
crash> files
PID: 1        TASK: ffff958bc0042f00  CPU: 15   COMMAND: "systemd"
ROOT: /    CWD: /
 FD       FILE            DENTRY           INODE       TYPE PATH
  0 ffff959bc1f00300 ffff953c59411a28 ffff955b3ee35a80 CHR  /dev/null
  1 ffff959bc1f00300 ffff953c59411a28 ffff955b3ee35a80 CHR  /dev/null
  2 ffff959bc1f00300 ffff953c59411a28 ffff955b3ee35a80 CHR  /dev/null
  3 ffff959bc3726000 ffff953c59411e60 ffff955b3ee36bf0 CHR  /dev/kmsg
  4 ffff959bc3726d00 ffff959bc0532798 ffff955bc0582580 UNKN [eventpoll]
  5 ffff959bc3725800 ffff959bc0533bd8 ffff955bc0582580 UNKN [signalfd]
  6 ffff959bc1fa5900 ffff958bc292dcb0 ffff958bc2935500 DIR  /sys/fs/cgroup/systemd/
  7 ffff958e035e4f00 ffff958c02cb6af8 ffff955bc0582580 UNKN [timerfd]
  8 ffff959bc3ada200 ffff95cbc0907e60 ffff955bc0582580 UNKN [eventpoll]
  9 ffff959bc3ad9e00 ffff958bc2ad8f30 ffff958bc2ad6f88 REG  /proc/1/mountinfo
 10 ffff959bc1fa4b00 ffff959bc0532948 ffff955bc0582580 UNKN inotify
 11 ffff959bc3ad8900 ffff959bc05355f0 ffff959bc0564230 SOCK NETLINK
 12 ffff959bc3adbb00 ffff95cbc0907950 ffff955bc0582580 UNKN inotify
 13 ffff959bc287c200 ffff958bc2ad8ca8 ffff958bc2ad0048 REG  /proc/swaps
 14 ffff959bc40e8800 ffff958bc05c05e8 ffff959bc2429630 SOCK NETLINK
 15 ffff959bc40eb100 ffff958bc05c06c0 ffff959bc242f0f0 SOCK UNIX
 16 ffff9567f8802e00 ffff955bc9cd5b00 ffff955bc0582580 UNKN inotify
 17 ffff9567f8800100 ffff955bc9cd4f30 ffff955bc0582580 UNKN inotify
 18 ffff95863fbe6f00 ffff958d98533d88 ffff9575bf019370 SOCK UNIX
 19 ffff959bc40eaa00 ffff958bc05c1290 ffff955bc0582580 UNKN [timerfd]
 22 ffff958bc2724800 ffff958bc2b560d8 ffff958bc2957bf0 SOCK UNIX
 24 ffff958bc2727300 ffff958bc2b56a20 ffff958bc29573b0 SOCK UNIX
 25 ffff958bc2724700 ffff958bc2b56000 ffff958bc2952ef0 SOCK UNIX
 26 ffff958bc2727c00 ffff958bc2b56510 ffff958bc2956070 SOCK UNIX
 27 ffff959bc3b64e00 ffff959bc07cc6c0 ffff95bbc56291d8 FIFO /run/dmeventd-server
 28 ffff959bc3b67f00 ffff959bc07cd7a0 ffff95bbc562a918 FIFO /run/dmeventd-client
 29 ffff959bc3b64800 ffff959bc07cc510 ffff959bc0509e70 SOCK UNIX
 30 ffff958bc2354e00 ffff95cbc0905878 ffff95cbc1cd4340 CHR  /dev/rfkill
 31 ffff959bc3b67600 ffff959bc0448510 ffff959bc0398350 CHR  /dev/autofs
 32 ffff958bc2720c00 ffff958bc2b56d80 ffff958bc29531b0 SOCK UNIX
 35 ffff958cd7d8c800 ffff9596a004aaf8 ffff9590ece1a3f0 SOCK UNIX
 36 ffff959576ffcb00 ffff953c7962af30 ffff95910ed552b0 SOCK UNIX
 43 ffff959bc1fe2600 ffff959bc243d290 ffff959bc050ac30 SOCK UNIX
 44 ffff958bcbf3f100 ffff958bca41c510 ffff958bc2ae2ef0 SOCK UNIX
 45 ffff959bc1fee400 ffff959bc04dc6c0 ffff959bc0508b30 SOCK NETLINK
 46 ffff958bcbf3e100 ffff958bd50f5cb0 ffff955bc0582580 UNKN [timerfd]
 47 ffff958bcbf3dd00 ffff95bbc4639878 ffff958bc2ae6070 SOCK UNIX
 49 ffff959bc3b67800 ffff959bc07cdbd8 ffff959bc050b730 SOCK UNIX
 50 ffff958bcbf3de00 ffff956bccd92000 ffff958bc2ae4a70 SOCK UNIX
 51 ffff959bc1fed900 ffff959bc24360d8 ffff95bbc562da80 FIFO /run/initctl
 52 ffff959bc1feea00 ffff959bc2434d80 ffff959bc050cff0 SOCK NETLINK
 53 ffff959bc1fee200 ffff959bc2435e60 ffff959bc050e330 SOCK NETLINK
 56 ffff956c3a03e700 ffff956c006d2bd0 ffff956bc8837930 SOCK UNIX
 57 ffff956c11380400 ffff956c005396c8 ffff956bc881eb70 SOCK UNIX
 58 ffff956c06e1d000 ffff958be4648af8 ffff956bc881f0f0 SOCK UNIX
 59 ffff959bc40ec700 ffff959bc06dd440 ffff955bc0681e70 SOCK UNIX
 65 ffff953c5f1cd900 ffff953c6318d1b8 ffff953c638efbf0 SOCK UNIX
 66 ffff953c5f1cc500 ffff955bc07e37a0 ffff953c638eca70 SOCK UNIX
 67 ffff956bc2f13200 ffff956bccd85d88 ffff956bc8b4e5f0 SOCK UNIX
 69 ffff956bc2f13100 ffff956bccd85440 ffff956bc8b490b0 SOCK UNIX
 74 ffff956bc2f11100 ffff958bc522aaf8 ffff956bc8b4e8b0 SOCK UNIX
 82 ffff956bc2f11800 ffff956bccdf8360 ffff956bc8b4daf0 SOCK UNIX
 84 ffff956bcb021100 ffff956bc0647bd8 ffff956bcce6b730 SOCK UNIX
 89 ffff956bc8dc3500 ffff956bc0647878 ffff956bcce6e330 SOCK UNIX
 90 ffff956bc280a000 ffff956bd1fd2f30 ffff956bc881e5f0 SOCK UNIX
 91 ffff956c3a337800 ffff956bcccdea20 ffff956bc882e330 SOCK UNIX
 92 ffff956c48b5ca00 ffff956c0befa5e8 ffff956bc8846330 SOCK UNIX
 93 ffff959bc1fec000 ffff959bc2435878 ffff959bc050f670 SOCK UNIX
 94 ffff959bc3b66100 ffff959bc06eca20 ffff959bc055c600 FIFO
 95 ffff959bc1feed00 ffff959bc24361b0 ffff959bc050c230 SOCK UNIX
 96 ffff95bbeb657100 ffff958c00b45cb0 ffff958be4416070 SOCK UNIX
 97 ffff958bc2723800 ffff958bc2b57cb0 ffff958bc29505b0 SOCK UNIX
105 ffff958bc2723500 ffff958bc2b56948 ffff958bc29565f0 SOCK UNIX
106 ffff958bcbf3e600 ffff958bd52f37a0 ffff958bc2ae3cb0 SOCK UNIX
```
