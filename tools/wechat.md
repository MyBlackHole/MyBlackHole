# wechat

```shell
paru -S wechat-universal-bwrap

sudo nvim /usr/share/applications/wechat-universal.desktop

env WECHAT_IME_WORKAROUND=fcitx

Exec=env WECHAT_IME_WORKAROUND=fcitx /usr/lib/wechat-universal/start.sh %u
```
