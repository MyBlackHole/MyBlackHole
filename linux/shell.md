# shell

## 参数
[$细节](https://zhuanlan.zhihu.com/p/57784678)
- **$\*** 和 **$@**
在 Bash 中没有双引号时, 它们两个被扩展后, 结果是一样的, 都是表示外部输入的参数列表.
当有双引号时, 如 “$*”, “$@”, 这个时候, 前者表示的是用 IFS (Internal Field Separator) 分隔符连接起来的统一字符, 后者则表示的是输入的每个参数.

例如:
```shell
#!/bin/bash

export IFS=%

cnt=1
for i in “$*”
do
    echo “Number of $cnt parameter is: $i”
    (( cnt++ ))
done

echo
echo 

cnt=1
for i in “$@”
do 
    echo “Number of $cnt parametre is: $i”
    (( cnt++ ))
done

# run
./test_1.sh “Hello, how are you?” Second Third Fourth

# out
Number of 1 parameter is: Hello, how are you?%Second%Third%Fourth


Number of 1 parameter is: Hello, how are you?
Number of 2 parameter is: Second
Number of 3 parameter is: Third
Number of 4 parameter is: Fourth
```

- $$, $!, $? 获得进程 ID 信息
$$ 获得当前进程 ID
$! 获得之前(上一个)进程 ID
$? 获得之前(上一个)进程结束的状态码 (0 表示成功, 1 表示失败)

例如:
```shell
#!/bin/bash

echo “Current process ID is: $$”

sleep 100 &
echo “The most recent process ID is: $!”
echo “The most recent process exit status is: $?”

# run
./test_2.sh

# out
Current process ID is: 15599
The most recent process ID is: 15600
The most recent process ID exit status is: 0
```

- $- 和 $_
$- 是 set 命令的 –h 和 –B 的参数, 表示使用内置的 set 命令扩展解释之后的参数行, 
   具体分别表示为, 记住工作路径, 和允许使用 ! 历史扩展, 详细请参阅 set 命令.

$_ (下划线) 表示的是打印上一个输入参数行, 当这个命令在开头时, 打印输出文档的绝对路径名.

```shell
#!/bin/bash
echo “Current absolute file path name is: $_”
echo “$-“
echo “Second $_”

let cnt=1
echo “Third $_”
echo “$cnt”
echo “Fourth $_”

# run
./test_3.sh

# out
Current absolute file path name is: ./test_3.sh
hB
Second hB
Third cnt=1
1
Fourth 1
```

- 具体每个选项对应的判断内容
```shell
-e filename 如果 filename存在，则为真 
-d filename 如果 filename为目录，则为真 
-f filename 如果 filename为常规文件，则为真 
-L filename 如果 filename为符号链接，则为真 
-r filename 如果 filename可读，则为真 
-w filename 如果 filename可写，则为真 
-x filename 如果 filename可执行，则为真 
-s filename 如果文件长度不为0，则为真 
-h filename 如果文件是软链接，则为真
```

- shell 导入 xx.env
```shell
while read line; do export $line; done < xx.env
# 或
export $(xargs <.env)
```
