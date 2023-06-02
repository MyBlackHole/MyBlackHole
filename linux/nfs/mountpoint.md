# mountpoint
其功能是用于判断目录是否为挂载点
检查目录或文件是否为挂载点。

## 语法格式
```shell
mountpoint [参数] 目录或设备名
```


### 常用参数
-d: 显示文件系统的主、次设备号
-q: 静默执行模式
-x: 显示块设备的主、次设备号

## 例如
```shell
mountpoint [-qd] /目录/的/路径
mountpoint -x /dev/设备
```

