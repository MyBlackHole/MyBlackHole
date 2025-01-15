# deno

## install

```shell
paru -S deno

export PATH="/home/black/.deno/bin:$PATH"

# deno
DENO_INSTALL=$HOME/.deno
if [ -d "$DENO_INSTALL" ]; then
    export PATH=$DENO_INSTALL/bin:$PATH
fi
```

## run

- install npm packages
```shell
deno install npm://<package_name> -g
```

- android build
```shell
paru -S android-sdk android-ndk android-studio android-emulator
paru -S rustup
rustup defalut stable

deno task tauri android init
sudo archlinux-java set java-17-openjdk
```
