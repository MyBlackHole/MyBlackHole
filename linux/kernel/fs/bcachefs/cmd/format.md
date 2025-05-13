# format

```shell
❯ ./bcachefs format /dev/sdb
/dev/sdb contains a bcachefs filesystem
Proceed anyway? (y,n) y
External UUID:                             47de9857-139d-45de-a1a4-dc2f1e5b14f0
Internal UUID:                             02a14ea8-e8eb-4650-9046-0f5f389fc5d1
Magic number:                              c68573f6-66ce-90a9-d96a-60cf803df7ef
Device index:                              0
Label:
Version:        1.7: mi_btree_bitmap
Version upgrade complete:       0.0: (unknown version)
Oldest version on disk:                    1.7: mi_btree_bitmap
Created:                                   Sat Jun  1 14:30:02 2024
Sequence number:                           0
Time of last write:                        Thu Jan  1 08:00:00 1970
Superblock size:                           976 B/1.00 MiB
Clean:                                     0
Devices:                                   1
Sections:                                  members_v1,members_v2
Features:                                  new_siphash,new_extent_overwrite,btree_ptr_v2,extents_above_btree_updates,btree_updates_journalled,new_varint,journal_no_flush,alloc_v2,extents_across_btree_nodes
Compat features:

Options:
  block_size:                              512 B
  btree_node_size:                         256 KiB
  errors:                                  continue [ro] panic
  metadata_replicas:                       1
  data_replicas:                           1
  metadata_replicas_required:              1
  data_replicas_required:                  1
  encoded_extent_max:                      64.0 KiB
  metadata_checksum:                       none [crc32c] crc64 xxhash
  data_checksum:                           none [crc32c] crc64 xxhash
  compression:                             none
  background_compression:                  none
  str_hash:                                crc32c crc64 [siphash]
  metadata_target:                         none
  foreground_target:                       none
  background_target:                       none
  promote_target:                          none
  erasure_code:                            0
  inodes_32bit:                            1
  shard_inode_numbers:                     1
  inodes_use_key_cache:                    1
  gc_reserve_percent:                      8
  gc_reserve_bytes:                        0 B
  root_reserve_percent:                    0
  wide_macs:                               0
  acl:                                     1
  usrquota:                                0
  grpquota:                                0
  prjquota:                                0
  journal_flush_delay:                     1000
  journal_flush_disabled:                  0
  journal_reclaim_delay:                   100
  journal_transaction_names:               1
  version_upgrade:                         [compatible] incompatible none
  nocow:                                   0

members_v2 (size 160):
Device:                                    0
  Label:                                   (none)
  UUID:                                    1c71b9d1-530b-4917-b6d3-4cc88837d382
  Size:                                    28.7 GiB
  read errors:                             0
  write errors:                            0
  checksum errors:                         0
  seqread iops:                            0
  seqwrite iops:                           0
  randread iops:                           0
  randwrite iops:                          0
  Bucket size:                             256 KiB
  First bucket:                            0
  Buckets:                                 117360
  Last mount:                              (never)
  Last superblock write:                   0
  State:                                   rw
  Data allowed:                            journal,btree,user
  Has data:                                (none)
  Btree allocated bitmap blocksize:        1.00 B
  Btree allocated bitmap:                  0000000000000000000000000000000000000000000000000000000000000000
  Durability:                              1
  Discard:                                 0
  Freespace initialized:                   0
mounting version 1.7: mi_btree_bitmap opts=noverbose
initializing new filesystem
going read-write
initializing freespace
shutdown complete, journal seq 16
```


```shell
❯ gdb --args ./bcachefs format Debug/1G
(gdb) break bch2_fs_initialize
Breakpoint 1 at 0x1a69d0: file libbcachefs/recovery.c, line 1116.
(gdb) r
Starting program: /run/media/black/Data/Documents/github/c/bcachefs-tools/bcachefs format Debug/1G

This GDB supports auto-downloading debuginfo from the following URLs:
  <https://debuginfod.archlinux.org>
Enable debuginfod for this session? (y or [n]) y
Debuginfod has been enabled.
To make this setting permanent, add 'set debuginfod enabled on' to .gdbinit.
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/usr/lib/libthread_db.so.1".
[New Thread 0x7ffff7fba680 (LWP 191366)]
[New Thread 0x7ffff7fb1680 (LWP 191367)]
[New Thread 0x7ffff7fa8680 (LWP 191368)]
[New Thread 0x7ffff7f9f680 (LWP 191369)]
[New Thread 0x7ffff7d26680 (LWP 191370)]
[New Thread 0x7ffff7d1d680 (LWP 191371)]
[New Thread 0x7ffff7d14680 (LWP 191372)]
[New Thread 0x7ffff7d0b680 (LWP 191373)]
[Detaching after vfork from child process 191374]
Debug/1G contains a bcachefs filesystem
Proceed anyway? (y,n) y
External UUID:                             c526016a-541a-4262-9523-ff6963e1e680
Internal UUID:                             7e17e3e6-17a9-4882-836b-a7fbb967d34a
Magic number:                              c68573f6-66ce-90a9-d96a-60cf803df7ef
Device index:                              0
Label:                                     (none)
Version:                                   1.27: fast_device_removal
Incompatible features allowed:             1.27: fast_device_removal
Incompatible features in use:              0.0: (unknown version)
Version upgrade complete:                  0.0: (unknown version)
Oldest version on disk:                    1.27: fast_device_removal
Created:                                   Sat May 10 16:36:42 2025
Sequence number:                           0
Time of last write:                        Thu Jan  1 08:00:00 1970
Superblock size:                           976 B/1.00 MiB
Clean:                                     0
Devices:                                   1
Sections:                                  members_v1,members_v2
Features:                                  new_siphash,new_extent_overwrite,btree_ptr_v2,extents_above_btree_updates,btree_updates_journalled,new_varint,journal_no_flush,alloc_v2,extents_across_btree_nodes,incompat_version_field
Compat features:

Options:
  block_size:                              512 B
  btree_node_size:                         256 KiB
  errors:                                  continue [fix_safe] panic ro
  write_error_timeout:                     30
  metadata_replicas:                       1
  data_replicas:                           1
  metadata_replicas_required:              1
  data_replicas_required:                  1
  encoded_extent_max:                      64.0 KiB
  metadata_checksum:                       none [crc32c] crc64 xxhash
  data_checksum:                           none [crc32c] crc64 xxhash
  checksum_err_retry_nr:                   3
  compression:                             none
  background_compression:                  none
  str_hash:                                crc32c crc64 [siphash]
  metadata_target:                         none
  foreground_target:                       none
  background_target:                       none
  promote_target:                          none
  erasure_code:                            0
  casefold:                                0
  inodes_32bit:                            1
  shard_inode_numbers_bits:                0
  inodes_use_key_cache:                    1
  gc_reserve_percent:                      8
  gc_reserve_bytes:                        0 B
  root_reserve_percent:                    0
  wide_macs:                               0
  promote_whole_extents:                   1
  acl:                                     1
  usrquota:                                0
  grpquota:                                0
  prjquota:                                0
  degraded:                                [ask] yes very no
  journal_flush_delay:                     1000
  journal_flush_disabled:                  0
  journal_reclaim_delay:                   100
  journal_transaction_names:               1
  allocator_stuck_timeout:                 30
  version_upgrade:                         [compatible] incompatible none
  nocow:                                   0
  rebalance_on_ac_only:                    0

members_v2 (size 160):
Device:                                    0
  Label:                                   (none)
  UUID:                                    3d69cc45-c53f-4c3b-b4e8-caed7a21d9cd
  Size:                                    1.00 GiB
  read errors:                             0
  write errors:                            0
  checksum errors:                         0
  seqread iops:                            0
  seqwrite iops:                           0
  randread iops:                           0
  randwrite iops:                          0
  Bucket size:                             256 KiB
  First bucket:                            0
  Buckets:                                 4096
  Last mount:                              (never)
  Last superblock write:                   0
  State:                                   rw
  Data allowed:                            journal,btree,user
  Has data:                                (none)
  Btree allocated bitmap blocksize:        1.00 B
  Btree allocated bitmap:                  0000000000000000000000000000000000000000000000000000000000000000
  Durability:                              1
  Discard:                                 1
  Freespace initialized:                   0
  Resize on mount:                         0
[New Thread 0x7ffff7cac680 (LWP 191380)]
Using encoding defined by superblock: utf8-12.1.0
starting version 1.27: fast_device_removal

Thread 1 "bcachefs" hit Breakpoint 1, bch2_fs_initialize (c=0x7ffff7cae000) at libbcachefs/recovery.c:1116
1116    {
(gdb) bt
#0  bch2_fs_initialize (c=0x7ffff7cae000) at libbcachefs/recovery.c:1116
#1  0x00005555557245b6 in bch2_fs_start (c=c@entry=0x7ffff7cae000) at libbcachefs/super.c:1175
#2  0x0000555555727e3a in bch2_fs_open (devices=devices@entry=0x7fffffffccd0, opts=opts@entry=0x7fffffffca30) at libbcachefs/super.c:2376
#3  0x00005555555fb56a in cmd_format (argc=<optimized out>, argv=0x555555b656a0) at c_src/cmd_format.c:296
#4  0x00005555555d5738 in bcachefs::handle_c_command (argv=..., symlink_cmd=...) at src/bcachefs.rs:49
#5  0x00005555555d62bb in bcachefs::main () at src/bcachefs.rs:116
(gdb)

```
