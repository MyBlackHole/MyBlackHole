# break

## 用法

```shell
break [break-args] if (condition)

break main if argc > 1
break 180 if (string == NULL && i < 0)
break test.c:34 if (x & y) == 1
break myfunc if i % (j + 3) != 0
break 44 if strlen(mystring) == 0
```

## 案例

- 指定文件下断点
```shell
# 相对路径
break sys_socket_learn/shutdown_test.c:66
```

- 条件断点
```shell
break main.c:288 if (count == 48)
```
