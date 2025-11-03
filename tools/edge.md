# edge

- 强制开启密码保存
```shell
edge://flags/#enable-password-force-saving

Show autofill signatures: true
```

- 配置中文
```shell
❯ locale -a
C
C.utf8
POSIX
zh_CN.utf8

<!-- sudo nvim /usr/bin/microsoft-edge-stable -->
<!-- export LANGUAGE=zh_CN.utf8 -->

nvim ~/.config/microsoft-edge-stable-flags.conf
--ozone-platform=wayland
--enable-wayland-ime
--wayland-text-input-version=3

```


