# status

```shell
bash-4.4$ ceph --status
  cluster:
    id:     21785c43-dfef-4b46-82ca-1b74e4307056
    health: HEALTH_WARN
            mons a,b,d are low on available space
            Reduced data availability: 58 pgs inactive
            OSD count 0 < osd_pool_default_size 3

  services:
    mon: 3 daemons, quorum a,b,d (age 21h)
    mgr: a(active, since 2h), standbys: b
    osd: 0 osds: 0 up, 0 in

  data:
    pools:   9 pools, 58 pgs
    objects: 0 objects, 0 B
    usage:   0 B used, 0 B / 0 B avail
    pgs:     100.000% pgs unknown
             58 unknown
```
