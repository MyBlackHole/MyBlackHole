# lsns
lsns命令列出有关所有当前可访问的名称空间或给定名称空间的信息
命名空间标识符是一个索引节点号
## 语法
lsns [参数]

常用参数：

-J	使用 JSON 输出格式
-l	使用列表格式的输出
-n	不打印标题
-r	使用原生输出格式
-u	不截断列中的文本
-t	名字空间类型(mnt, net, ipc, user, pid, uts, cgroup)

## 例子

- 列出网络命名空间
```shell
lsns -t net
```
