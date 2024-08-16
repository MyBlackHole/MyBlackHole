# atuin

- 在线
```shell
bash <(curl https://raw.githubusercontent.com/ellie/atuin/main/install.sh)
 
atuin register -u MBlackHole -e MBlackHole@gmail.com -p MBlackHole
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
MBlackHole: MBlackHole
MBlackHole@gmail.com
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
Username: MBlackHole
```

- 导入本地历史到 atuin
```shell
atuin import auto
```

- 登陆
```shell
atuin login -u MBlackHole -p MBlackHole -k MBlackHole
```

- 配置登录时自动同步
```shell
nvim ~/.config/atuin/config.toml
auto_sync = true
```

- 配置同步频率 (deamon)
```shell
nvim ~/.config/atuin/config.toml
sync_frequency = "1h"
```
