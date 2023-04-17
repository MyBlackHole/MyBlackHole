# teledb 控制台
```shell
cd /teledb/telemonitor-stg/telemonitor/python/include
./decryptnew -t decode -e "Ti84oTg1" | grep 'Decrypted_string' | awk '{print $2}'
mycli -h 192.168.90.204 -P 9095 -uteledb -pteledb
```
