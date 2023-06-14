- Tue Jun 13 18:33:04 2023 - *** uWSGI listen queue of socket "127.0.0.1:45816" (fd: 3) full !!! (101/100) ***
```ini
# 这是由于uWSGI的监听队列满了，默认的监听队列长度为100，需要在uWSGI.ini文件中设置新的长度
[uwsgi]
listen = 500
```
