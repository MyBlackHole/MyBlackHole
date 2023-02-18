用法：parted [选项]... [设备 [命令 [参数]...]...]
将命令带着参数应用于“设备”。如果没有给出“命令”，则以交互模式运行。

选项：
  -h, --help                      显示此求助信息
  -l, --list                      列出所有块设备的分区配置
  -m, --machine                   显示机器可解析的输出
  -s, --script                    从不提示用户
  -v, --version                   显示版本
  -a, --align=[none|cyl|min|opt]  新分区的对齐

命令：
  align-check 类型 N                         检查分区 N 是否为 (最小=min|最佳=opt) 对齐类型
  help [COMMAND]                           打印通用求助信息，或 COMMAND 的帮助
  mklabel,mktable LABEL-TYPE               创建新的磁盘卷标 (分区表)
  mkpart 分区类型 [文件系统类型] 起始点 结束点 创建一个分区
  name 编号 名称                           将指定“编号”的分区命名为“名称”
  print [devices|free|list,all|数字]        显示分区表、可用设备、剩余空间、所有分区或特殊分区
  quit                                     退出程序
  rescue 起始点 终止点                      挽救临近“起始点”、“终止点”的遗失的分区
  resizepart NUMBER END                    改变 NUMBER 的大小
  rm NUMBER                                删除编号为 NUMBER 的分区
  select 设备                              选择要编辑的设备
  disk_set 旗标 状态                       变更已选设备上的旗标
  disk_toggle [旗标]                       切换已选设备上的旗标状态
  set 编号 旗标 状态                       改变指定“编号”分区的旗标
  toggle [编号 [旗标]]                     切换“编号”分区上的“旗标”状态
  unit 单位                                设置缺省的“单位”
  version                                  显示目前 GNU Parted 的版本与版权信息
