# df

读取文件系统层信息

## 参数
用法：df [选项]... [文件]...
显示每个指定文件所在的文件系统的信息，默认是显示所有文件系统。

必选参数对长短选项同时适用。
  -a, --all             包含虚拟、重复和无法访问的文件系统
  -B, --block-size=大小  使用指定字节数的块。例如，'-BM' 将以
                           1,048,576 字节为单位显示大小。
                           参见 SIZE 格式。
  -h, --human-readable  以 1024 为基底显示大小（例如：1023M）
  -H, --si              以 1000 为基底显示大小（例如，1.1G）
  -i, --inodes          显示inode 信息而非块使用量
  -k                    即--block-size=1K
  -l, --local           只显示本机的文件系统
      --no-sync         取得使用量数据前不进行同步动作(默认)
      --output[=域列表]      使用给定域列表定义的输出格式，
                               或者在缺省情况下输出所有域。
  -P, --portability     使用 POSIX 兼容的输出格式
      --sync            取得使用量数据前先调用同步（sync）动作
      --total           省略所有对可用空间无显著影响的项并生成总计值
  -t, --type=类型       只显示指定文件系统为指定类型的信息
  -T, --print-type      显示文件系统类型
  -x, --exclude-type=类型   只显示文件系统不是指定类型的信息
  -v                    （忽略）
      --help            显示此帮助信息并退出
      --version         显示版本信息并退出

所显示的数值是来自 --block-size、DF_BLOCK_SIZE、BLOCK_SIZE
及 BLOCKSIZE 环境变量中第一个可用的 SIZE 单位。
否则，默认单位是 1024 字节(或是 512，若设定 POSIXLY_CORRECT 的话)。

The SIZE argument is an integer and optional unit (example: 10K is 10*1024).
Units are K,M,G,T,P,E,Z,Y (powers of 1024) or KB,MB,... (powers of 1000).
Binary prefixes can be used, too: KiB=K, MiB=M, and so on.

“域列表”是由逗号分隔的列表，指示需要包含在内的列。有效的域名称包括：
'source', 'fstype', 'itotal', 'iused', 'iavail', 'ipcent', 'size',
'used', 'avail', 'pcent', 'file' 和 'target'（请参考 info 信息页）。

## 例子
- 
```shell
df -k
```
