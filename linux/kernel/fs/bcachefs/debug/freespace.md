# free space

```shell
(gdb) bt
#0  bch2_dev_freespace_init (c=c@entry=0x7ffff7cae000, ca=ca@entry=0x555555c2e800,
    bucket_start=bucket_start@entry=0, bucket_end=4096)
    at libbcachefs/alloc_background.c:2296
#1  0x000055555561b539 in bch2_fs_freespace_init (c=c@entry=0x7ffff7cae000)
    at libbcachefs/alloc_background.c:2415
#2  0x00005555556fb153 in bch2_fs_initialize (c=0x7ffff7cae000)
    at libbcachefs/recovery.c:1190
#3  0x00005555557245b6 in bch2_fs_start (c=c@entry=0x7ffff7cae000)
    at libbcachefs/super.c:1175
#4  0x0000555555727e3a in bch2_fs_open (devices=devices@entry=0x7fffffffcd10,
    opts=opts@entry=0x7fffffffca70) at libbcachefs/super.c:2376
#5  0x00005555555fb56a in cmd_format (argc=<optimized out>, argv=0x555555b656a0)
    at c_src/cmd_format.c:296
#6  0x00005555555d5738 in bcachefs::handle_c_command (argv=..., symlink_cmd=...)
    at src/bcachefs.rs:49
#7  0x00005555555d62bb in bcachefs::main () at src/bcachefs.rs:116

(gdb) p ca.usage.d
$3 = {{buckets = 4050, sectors = 0, fragmented = 0}, {buckets = 13, sectors = 6152,
    fragmented = 504}, {buckets = 32, sectors = 16384, fragmented = 0}, {buckets = 1,
    sectors = 512, fragmented = 0}, {buckets = 0, sectors = 0, fragmented = 0}, {
    buckets = 0, sectors = 0, fragmented = 0}, {buckets = 0, sectors = 0,
    fragmented = 0}, {buckets = 0, sectors = 0, fragmented = 0}, {buckets = 0,
    sectors = 0, fragmented = 0}, {buckets = 0, sectors = 0, fragmented = 0}, {
    buckets = 0, sectors = 0, fragmented = 0}}


(gdb) bt
#0  bch2_fs_initialize (c=0x7ffff7cae000) at libbcachefs/recovery.c:1116
#1  0x00005555557245b6 in bch2_fs_start (c=c@entry=0x7ffff7cae000)
    at libbcachefs/super.c:1175
#2  0x0000555555727e3a in bch2_fs_open (devices=devices@entry=0x7fffffffcd10,
    opts=opts@entry=0x7fffffffca70) at libbcachefs/super.c:2376
#3  0x00005555555fb56a in cmd_format (argc=<optimized out>, argv=0x555555b656a0)
    at c_src/cmd_format.c:296
#4  0x00005555555d5738 in bcachefs::handle_c_command (argv=..., symlink_cmd=...)
    at src/bcachefs.rs:49
#5  0x00005555555d62bb in bcachefs::main () at src/bcachefs.rs:116

(gdb) p c.devs
$2 = {0x555555c2e800, 0x0 <repeats 63 times>}
(gdb) p c.devs[0]
$3 = (struct bch_dev *) 0x555555c2e800
(gdb) p c.devs[0].usage
$4 = (struct bch_dev_usage_full *) 0x555555b66600
(gdb) p c.devs[0].usage.d
$5 = {{buckets = 0, sectors = 0, fragmented = 0} <repeats 11 times>}
```
