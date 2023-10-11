# celery 

```shell
#  redis-cli -n 1 keys \* 
"_kombu.binding.celery.pidbox"
"_kombu.binding.celeryev"
"_kombu.binding.celery"
"default"

default: 是放任务的 (没有任务是不存在)
_kombu.binding.xxxx: 是工作队列

pidbox和celeryev是celery内部用来发送event和管理其他队列的队列
```
