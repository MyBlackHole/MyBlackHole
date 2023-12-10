# execslower

跟踪缓慢的 ext4 操作

```shell
sudo /usr/share/bcc/tools/execslower

Tracing ext4 operations slower than 10 ms
TIME     COMM           PID    T BYTES   OFF_KB   LAT(ms) FILENAME
11:59:13 upstart        2252   W 48      1          10.76 dbus.log
11:59:13 gnome-screensh 14993  R 144     0          10.96 settings.ini
11:59:13 gnome-screensh 14993  R 28      0          16.02 gtk.css
11:59:13 gnome-screensh 14993  R 3389    0          18.32 gtk-main.css
11:59:25 rs:main Q:Reg  1826   W 156     60         31.85 syslog
11:59:25 pool           15002  R 208     0          14.98 .xsession-errors
11:59:25 pool           15002  R 644     0          12.28 .ICEauthority
11:59:25 pool           15002  R 220     0          13.38 .bash_logout
11:59:27 dconf-service  2599   S 0       0          22.75 user.BHDKOY
11:59:33 compiz         2548   R 4096    0          19.03 firefox.desktop
11:59:34 compiz         15008  R 128     0          27.52 firefox.sh
11:59:34 firefox        15008  R 128     0          36.48 firefox
11:59:34 zeitgeist-daem 2988   S 0       0          62.23 activity.sqlite-wal
11:59:34 zeitgeist-fts  2996   R 8192    40         15.67 postlist.DB
11:59:34 firefox        15008  R 140     0          18.05 dependentlibs.list
11:59:34 zeitgeist-fts  2996   S 0       0          25.96 position.tmp
11:59:34 firefox        15008  R 4096    0          10.67 libplc4.so
11:59:34 zeitgeist-fts  2996   S 0       0          11.29 termlist.tmp
...
```
