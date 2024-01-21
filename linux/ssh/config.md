# config

- 设置代理
```shell
sudo apt install connect-proxy (提供 connect 命令支持)

lvim ~/.ssh/config
Host github.com ssh.github.com *github.*
ProxyCommand connect -S 127.0.0.1:1080 %h %p
```
