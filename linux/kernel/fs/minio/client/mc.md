# mc


ls        列出文件和文件夹。
mb        创建一个存储桶或一个文件夹。
cat       显示文件和对象内容。
pipe      将一个STDIN重定向到一个对象或者文件或者STDOUT。
share     生成用于共享的URL。
cp        拷贝文件和对象。
mirror    给存储桶和文件夹做镜像。
find      基于参数查找文件。
diff      对两个文件夹或者存储桶比较差异。
rm        删除文件和对象。
events    管理对象通知。
watch     监听文件和对象的事件。
anonymous 管理访问策略。
session   为cp命令管理保存的会话。
config    管理mc配置文件。
update    检查软件更新。
version   输出版本信息。

## config
- 补全
```shell
sudo wget https://raw.githubusercontent.com/minio/mc/master/autocomplete/bash_autocomplete -O /etc/bash_completion.d/mc
source /etc/bash_completion.d/mc
```

## admin

- 查看日志
```shell
mc admin logs --debug BHOSS
```

- 跟踪请求
```shell
mc admin trace -v BH
```

## mb
```shell
<!-- MC_REGION(指定区域) -->
MC_REGION="cn-east-1" mc --help
```

## 例子
```shell
git clone git@github.com:minio/mc.git
make
./mc alias set BH http://127.0.0.1:9000 Thww3fGoI8fO1H6tXKxa Y1dzBaoRsBj9BjARV3LWxyzoZ2SGy7QGnZXH9v6o


./mc ls BH
[2023-10-27 12:12:36 CST]     0B test/
```
