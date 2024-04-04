# coredumpctl



```shell
coredumpctl
TIME                          PID  UID  GID SIG     COREFILE EXE                                                                              SIZE
Thu 2024-04-04 08:36:36 CST 89240 1000 1000 SIGSEGV present  /run/media/black/Data/Documents/github/C/postgres/src/bin/pg_waldump/pg_waldump 32.7K
Thu 2024-04-04 08:40:03 CST 89521 1000 1000 SIGSEGV present  /run/media/black/Data/Documents/github/C/postgres/src/bin/pg_waldump/pg_waldump 32.7K
```

- gdb core
```shell
coredumpctl gdb 89521
```

- 保存 core
```shell
coredumpctl -o pg_waldump.dump dump 89521
```
