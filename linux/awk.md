# awk

## 例子

- 截取某一列
```shell
[root@node2 airflow]# zfs get type |grep volume
aiopool/192.168.78.213_1_single                type      volume      -
aiopool/192.168.78.213_1_single_log            type      volume      -

[root@node2 airflow]# zfs get type |grep volume | awk '{print $1}'
aiopool/192.168.78.213_1_single
aiopool/192.168.78.213_1_single_log
```
