- Tue Jun 13 18:33:04 2023 - *** uWSGI listen queue of socket "127.0.0.1:45816" (fd: 3) full !!! (101/100) ***
```ini
# 这是由于uWSGI的监听队列满了，默认的监听队列长度为100，需要在uWSGI.ini文件中设置新的长度
[uwsgi]
listen = 500
```

- /usr/bin/uwsgi: unrecognized option '--wsgi-file'
```shell
# 加上
--plugin python3
```

- 没有python_plugin.so
```shell
sudo apt-get install uwsgi-plugin-python3
```

- uwsgi ModuleNotFoundError: No module named 'flask'
```shell
环境没有指定
```
