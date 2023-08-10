- sqlalchemy.exc.TimeoutError: QueuePool limit of size 10 overflow 10 reached, connection timed out, timeout 30 (Background on this error at: http://sqlalche.me/e/13/3o7r)


- 'UserLock' object has no attribute '_sa_instance_state'
```shell
class UserLock(SQLModel):
to
class UserLock(SQLModel, table=True):
```
