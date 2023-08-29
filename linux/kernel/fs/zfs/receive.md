# receive
接收快照流

```shell
ssh root@xxxxxxxxxxxxxx -p 22 zfs send -I aiopool/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx@1690788378 aiopool/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx1690863820 | zfs receive -F aiopool/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```
