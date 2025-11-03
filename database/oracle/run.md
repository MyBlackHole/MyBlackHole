# run

```shell
podman pull registry.cn-hangzhou.aliyuncs.com/zhuyijun/oracle:19c

mkdir -p /mydata/oracle/oradata
chmod 777 /mydata/oracle/oradata

podman run -d \
 -p 1521:1521 -p 5500:5500 \
 -e ORACLE_SID=ORCLCDB \
 -e ORACLE_PDB=ORCLPDB1 \
 -e ORACLE_PWD=YourPassword123 \
 -e ORACLE_CHARACTERSET=AL32UTF8 \
 -v /mydata/oracle/oradata:/opt/oracle/oradata \
 --name oracle19c \
 registry.cn-hangzhou.aliyuncs.com/zhuyijun/oracle:19c

docker logs -f oracle19c
```
