# docker 

```shell
# deploy mini instance
docker run -p 2881:2881 --name oceanbase-ce -d oceanbase/oceanbase-ce

# deploy an instance of the slim size according to the current container
docker run -p 2881:2881 --name oceanbase-ce -e MODE=slim -e OB_MEMORY_LIMIT=5G -v {init_sql_folder_path}:/root/boot/init.d -d oceanbase/oceanbase-ce

# deploy an instance of the largest size according to the current container
docker run -p 2881:2881 --name oceanbase-ce -e MODE=normal -d oceanbase/oceanbase-ce

# deploy a quick-start instance in any mode as desired to the current container
docker run -p 2881:2881 --name oceanbase-ce -e FASTBOOT=true -d oceanbase/oceanbase-ce
```

```shell
$ docker logs oceanbase-ce | tail -1
boot success!
```

```shell
docker exec -it oceanbase-ce ob-mysql sys # Connect to sys tenant
docker exec -it oceanbase-ce ob-mysql root # Connect to the root account of a general tenant
docker exec -it oceanbase-ce ob-mysql test # Connect to the test account of a general tenant

mysql -uroot -h127.1 -P2881
```
