# execsnoop

通过 exec() 系统调用跟踪新进程

```shell
sudo /usr/share/bcc/tools/execsnoop

PCOMM            PID    PPID   RET ARGS
gnome-screensho  14882  14881    0 /usr/bin/gnome-screenshot --gapplication-service
systemd-hostnam  14892  1        0 /lib/systemd/systemd-hostnamed
nautilus         14897  2767    -2 /home/tecmint/bin/net usershare info
nautilus         14897  2767    -2 /home/tecmint/.local/bin/net usershare info
nautilus         14897  2767    -2 /usr/local/sbin/net usershare info
nautilus         14897  2767    -2 /usr/local/bin/net usershare info
nautilus         14897  2767    -2 /usr/sbin/net usershare info
nautilus         14897  2767    -2 /usr/bin/net usershare info
nautilus         14897  2767    -2 /sbin/net usershare info
nautilus         14897  2767    -2 /bin/net usershare info
nautilus         14897  2767    -2 /usr/games/net usershare info
nautilus         14897  2767    -2 /usr/local/games/net usershare info
nautilus         14897  2767    -2 /snap/bin/net usershare info
compiz           14899  14898   -2 /home/tecmint/bin/libreoffice --calc
compiz           14899  14898   -2 /home/tecmint/.local/bin/libreoffice --calc
compiz           14899  14898   -2 /usr/local/sbin/libreoffice --calc
compiz           14899  14898   -2 /usr/local/bin/libreoffice --calc
compiz           14899  14898   -2 /usr/sbin/libreoffice --calc
libreoffice      14899  2252     0 /usr/bin/libreoffice --calc
dirname          14902  14899    0 /usr/bin/dirname /usr/bin/libreoffice
basename         14903  14899    0 /usr/bin/basename /usr/bin/libreoffice
...
```
