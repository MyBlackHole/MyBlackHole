# dd

## 描述

### 语法

|  参数 | 说明   |
|-------------- | -------------- |
|if |文件名：输入文件名，默认为标准输入。即指定源文件。 |
|of |文件名：输出文件名，默认为标准输出。即指定目的文件。 |
|ibs |bytes：一次读入bytes个字节，即指定一个块大小为bytes个字节。 |
|obs |bytes：一次输出bytes个字节，即指定一个块大小为bytes个字节。 |
|bs |bytes：同时设置读入/输出的块大小为bytes个字节。 |
|cbs |bytes：一次转换bytes个字节，即指定转换缓冲区大小。 |
|skip |blocks：从输入文件开头跳过blocks个块后再开始复制。 |
|seek |blocks：从输出文件开头跳过blocks个块后再开始复制。 |
|count |blocks：仅拷贝blocks个块，块大小等于ibs指定的字节数。 |

### 例子
- 镜像写入 
```
dd if=.iso of=/dev/sda2 # 写入镜像 
dd if=boot.img of=/dev/fd0 bs=1440k  # Linux 下制作启动盘 
```
- img
```shell
dd if=/dev/zero of=/opt/disk.img bs=1024k count=512
```

- 批量随机生成文件
```shell
tmp_dir=./temp
mkdir $tmp_dir

for i in {1..1040};do
    echo $tmp_dir/${i}.log
    dd if=/dev/zero of=$tmp_dir/${i}.log bs=`shuf -n 1 -i 0-16`k count=1 &>/dev/null
done
```

- 读取指定 501 块号的数据
```shell
dd if=./base/17292/17040 of=./17040-501.img count=1 bs=8192 skip=500
```

- 读取指定 501 块号的数据并写入到指定文件 502 块号
```shell
将test 文件写入nvme0n1， 跳过seek数值的区块，从10000开始写，单次512bytes, 一共写10000次
dd if=./17040-501.img of=./base/17292/17040 bs=8192 seek=501 count=1 conv=notrunc
```
