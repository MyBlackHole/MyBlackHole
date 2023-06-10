# inotify

## 参数
```shell
-m : 始终保持事件监听 

-r : 递归监控目录数据信息变化 

-q : 输出信息少（只打印事件信息） 

-timefmt : 指定时间输出格式 

-format : 打印使用指定的输出类似格式字符串；即实际监控输出内容 

-e : 指定监听指定的事件，如果省略，表示所有事件都进行监听 
    close_write : 文件或目录关闭，在写入模式打开之后关闭的。 

    move : 文件或目录不管移动到或是移出监控目录都触发事件 

    create : 文件或目录创建在监控目录中 

    delete : 文件或目录被删除在监控目录中 

```

## 例子
- 监控test文件夹
```shell
/usr/bin/inotifywait -mrq --timefmt '%d/%m/%y %H:%M' \
    --format '%T %w %f %e' -e create,delete,modify,attrib \
    test
```
