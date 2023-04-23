# uwsgi
wsgi 的具体实现

- 启动
```shell
uwsgi --ini xxx.ini
```

- 重启
```shell
uwsgi --reload xxx.ini
```

- 停止
```shell
uwsgi --stop xxx.ini
```

- 强制关闭 [[killall]]
```shell
killall -s INT uwsgi
```
