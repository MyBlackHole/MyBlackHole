# null_dev
![[imgs/Pasted image 20230702133344.png]]

- 启用写入
```shell
sudo modprobe null_blk queue_mode=2 nr_devices=1 memory_backed=1 gb=1
sudo modprobe null_blk queue_mode=2 nr_devices=2 memory_backed=1 gb=1

sudo mount -t xfs /dev/nullb0 /media/black/nullb

lsblk -a
nullb0      252:0    0     1G  0 disk
```
