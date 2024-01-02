# btrfs

- 给根创建快照
```shell
❯ sudo btrfs subvolume snapshot / /snap/root
Create a snapshot of '/' in '/snap/root'
```

- 查看指定路径快照
```shell
❯ sudo btrfs subvolume list /
ID 256 gen 1396 top level 5 path @
ID 257 gen 1396 top level 5 path @home
ID 259 gen 1396 top level 256 path snap/root
```
