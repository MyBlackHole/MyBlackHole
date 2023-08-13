# bookmark
标签,替代快照存在,为了增量发送留埋点
空间占用小(记录 guid\createtxg)
createtxg 全局唯一永久递增

```shell
root@black:/sandisk# zfs get all sandisk#1
NAME       PROPERTY              VALUE                   SOURCE
sandisk#1  type                  bookmark                -
sandisk#1  creation              日 8月 13 18:25 2023  -
sandisk#1  referenced            155M                    -
sandisk#1  createtxg             7242                    -
sandisk#1  guid                  6320954441941772454     - (全局唯一标识)
sandisk#1  logicalreferenced     155M                    -  (逻辑产考大小)
```

- 显示所有标签
```shell
root@black:/sandisk# sudo zfs list -t bookmark
NAME        USED  AVAIL     REFER  MOUNTPOINT
sandisk#1      -      -      155M  -
```

- 给快照生成标签
```shell
zfs bookmark sandisk@1 sandisk#1
```

- 删除标签
```shell
zfs destroy sandisk#1
```
