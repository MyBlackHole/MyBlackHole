# 防火墙

- 删除规则
```shell
sudo ufw delete 7 # 编号7
sudo ufw delete allow 53
```

- 查询规则状态
```shell
sudo ufw status numbered # numbered 显示编号
```

- 拒绝 外部访问
```shell
sudo ufw deny smtp
```

- 允许特定 ip
```shell
sudo ufw allow from 192.168.254.254
```
