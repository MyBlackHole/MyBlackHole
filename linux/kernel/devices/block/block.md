# block
[不错的文章](https://blog.csdn.net/hu1610552336/article/details/111464548)
[不错的文章](https://zhuanlan.zhihu.com/p/507214979)
[不错的文章](https://zhuanlan.zhihu.com/p/140916092)

- null_blk 使用
```shell
# 创建
sudo modprobe null_blk nr_devices=1 zoned=1 zone_nr_conv=4 zone_size=64
# 或
sudo modprobe null_blk nr_devices=1 zoned=1

# 查询
lsblk -a
nullb0      252:0    0   250G  0 disk

# 删除
sudo rmmod null_blk
```
