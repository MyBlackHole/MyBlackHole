# volume

- 删除所有 dangling (悬挂) 数据卷
```shell
docker volume rm $(docker volume ls -qf dangling=true)
```

