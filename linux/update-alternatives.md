# update-alternatives

进行版本切换

![[imgs/Pasted image 20230428022352.png]]

```shell
$ update-alternatives --help
用法：update-alternatives [<选项> ...] <命令>

命令：
  --install <链接> <名称> <路径> <优先级>
    [--slave <链接> <名称> <路径>] ...
                           在系统中加入一组候选项。
  --remove <名称> <路径>   从 <名称> 替换组中去除 <路径> 项。
  --remove-all <名称>      从替换系统中删除 <名称> 替换组。
  --auto <名称>            将 <名称> 的主链接切换到自动模式。
  --display <名称>         显示关于 <名称> 替换组的信息。
  --query <名称>           机器可读版的 --display <名称>.
  --list <名称>            列出 <名称> 替换组中所有的可用候选项。
  --get-selections         列出主要候选项名称以及它们的状态。
  --set-selections         从标准输入中读入候选项的状态。
  --config <名称>          列出 <名称> 替换组中的可选项，并就使用其中哪一个，征询用户的意见。
  --set <名称> <路径>      将 <路径> 设置为 <名称> 的候选项。
  --all                    对所有可选项一一调用 --config 命令。

<链接> 是指向 /etc/alternatives/<名称> 的符号链接。(如 /usr/bin/pager)
<名称> 是该链接替换组的主控名。(如 pager)
<路径> 是候选项目标文件的位置。(如 /usr/bin/less)
<优先级> 是一个整数，在自动模式下，这个数字越高的选项，其优先级也就越高。
..........
```

## install

```shell
# 添加 python link
sudo update-alternatives --install /usr/bin/python python /usr/bin/python2.7 2
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.11 0

# 第一个参数: --install 表示向update-alternatives注册服务名。
# 第二个参数: 注册最终地址，成功后将会把命令在这个固定的目的地址做真实命令的软链，以后管理就是管理这个软链；
# 第三个参数: 服务名，以后管理时以它为关联依据。
# 第四个参数: 被管理的命令绝对路径。
# 第五个参数: 优先级，数字越大优先级越高。
```

## remove

```shell
update-alternatives –remove python /usr/bin/python2.7
```

## display

```shell
update-alternatives --display python 
python - 手动模式
link best version is /usr/bin/python3.5
链接目前指向 /usr/bin/python2.7
link python is /usr/bin/python
/usr/bin/python2.7 - 优先级 1
/usr/bin/python3.5 - 优先级 2
```

## 例子

- 修改 gcc
```shell
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 100
sudo update-alternatives --config gcc
```

- 修改默认浏览器
```shell
sudo update-alternatives --config x-www-browser
```

- 修改 python
```shell
sudo update-alternatives --config python
update-alternatives: 警告: 候选项 /usr/bin/python3.10(链接组 python 的一部分)不存在；从候选项列表中移除
update-alternatives: 警告: /etc/alternatives/python 正处于未决状态；将以最佳选项更新它
有 1 个候选项可用于替换 python (提供 /usr/bin/python)。

  选择       路径              优先级  状态
------------------------------------------------------------
  0            /usr/bin/python3.8   1         自动模式
  1            /usr/bin/python3.8   1         手动模式

要维持当前值[*]请按<回车键>，或者键入选择的编号：0
update-alternatives: 使用 /usr/bin/python3.8 来在自动模式中提供 /usr/bin/python (python)
```
