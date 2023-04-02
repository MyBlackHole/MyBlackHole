# pyinotify
监控文件修改

事件标志|    事件含义
IN_ACCESS|    被监控项目或者被监控目录中的文件被访问，比如一个文件被读取
IN_MODIFY|    被监控项目或者被监控目录中的文件被修改
IN_ATTRIB|    被监控项目或者被监控目录中的文件的元数据被修改
IN_CLOSE_WRITE|    一个打开切等待写入的文件或者目录被关闭
IN_CLOSE_NOWRITE|    一个以只读方式打开的文件或者目录被关闭
IN_OPEN|    文件或者目录被打开
IN_MOVED_FROM|    被监控项目或者目录中的文件被移除监控区域
IN_MOVED_TO|    文件或目录被移入监控区域
IN_CREATE|    在所监控的目录中创建子目录或文件
IN_DELETE|    在所监控的目录中删除目录或文件
IN_CLOSE*|    文件被关闭,等同于IN_CLOSE_WRITE*
IN_MOVE|    文件被移动,等同于IN_CLOSE_NOWRITE

# 安装
```shell
sudo pip install pyinotify
```


## 例子

- 健康 postgresql 数据文件修改
```shell
sudo python3 -m pyinotify -v /var/lib/postgresql/14/main
```
