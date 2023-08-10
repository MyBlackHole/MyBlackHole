```shell
<!-- win -->
GET-CimInstance -query "SELECT * from Win32_DiskDrive"

wsl --mount \\.\PHYSICALDRIVE1

<!-- wsl linux -->
sudo mount /dev/sdd1 /media/black/Data
```
