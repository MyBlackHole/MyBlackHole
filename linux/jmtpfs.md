#

## 安装
fusermount[[fusermount]] 卸载
```shell
sudo apt install jmtpfs
```

## 使用
- 挂载手机设备
```shell
jmtpfs ~/mnt
```

- 列出所有设备
```shell
jmtpfs -l
```

- 指定总线设备挂载
```shell
sudo jmtpfs -o allow_other -device=1,8 /media/black/MTP 
```
