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

- 查看详情
```shell
coredumpctl info 2345
           PID: 588797 (fs-cli)
           UID: 1000 (black)
           GID: 1000 (black)
        Signal: 7 (BUS)
     Timestamp: Thu 2024-06-06 13:38:31 CST (1h 44min ago)
  Command Line: ./fs-cli --version
    Executable: /run/media/black/Data/Documents/aio/aio-tools/fs-backup/fsclient/fs-cli
 Control Group: /user.slice/user-1000.slice/user@1000.service/tmux-spawn-13aacecc-798c-4a22-9326-5da20d826feb.scope
          Unit: user@1000.service
     User Unit: tmux-spawn-13aacecc-798c-4a22-9326-5da20d826feb.scope
         Slice: user-1000.slice
     Owner UID: 1000 (black)
       Boot ID: 1827be70bda44e3b9598c4aaf5ac32bc
    Machine ID: f10031e549f446d2963affd5647cc441
      Hostname: black
       Storage: /var/lib/systemd/coredump/core.fs-cli.1000.1827be70bda44e3b9598c4aaf5ac32bc.588797.1717652311000000.zst (present)
  Size on Disk: 60.5K
       Message: Process 588797 (fs-cli) of user 1000 dumped core.

                Stack trace of thread 588797:
                #0  0x000070ef250faf07 clock_nanosleep (libc.so.6 + 0xdef07)
                #1  0x000070ef25106d77 __nanosleep (libc.so.6 + 0xead77)
                #2  0x000070ef25118e51 sleep (libc.so.6 + 0xfce51)
                #3  0x00005a03100f669a args_process (fs-cli + 0xb69a)
                #4  0x00005a03100f72f3 main (fs-cli + 0xc2f3)
                #5  0x000070ef25041c88 n/a (libc.so.6 + 0x25c88)
                #6  0x000070ef25041d4c __libc_start_main (libc.so.6 + 0x25d4c)
                #7  0x00005a03100f5bb5 _start (fs-cli + 0xabb5)
                ELF object binary architecture: AMD x86-64
```

- 转储 core
```shell
coredumpctl -o core.gz dump 89521
```
