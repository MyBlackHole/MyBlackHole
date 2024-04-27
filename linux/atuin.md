# atuin

- 在线
```shell
bash <(curl https://raw.githubusercontent.com/ellie/atuin/main/install.sh)
 
atuin register -u <USERNAME> -e <EMAIL> -p <PASSWORD>
atuin import auto
atuin sync
```

- 离线
```shell
bash <(curl https://raw.githubusercontent.com/ellie/atuin/main/install.sh)

atuin import auto
```

- 账号
```shell
BlackHole: 密码(你猜)
myisblackhole@gmail.com
```

- 状态
```shell
<!-- atuin status -->
Atuin v18.2.0 - Build rev NO_GIT

[Local]
Sync frequency: 10m
Last sync: 2024-04-19 1:59:58.600137885 +00:00:00
History count: 944
Deleted history count: 0

[Remote]
Address: https://api.atuin.sh
Username: BlackHole
```

- 导入本地历史到 atuin
```shell
atuin import auto
```
