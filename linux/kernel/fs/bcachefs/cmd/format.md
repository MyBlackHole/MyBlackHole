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
