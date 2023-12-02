# crun

```shell
git clone git@github.com:containers/crun.git
cd crun
./autogen.sh
./configure CFLAGS='-g -O0' --prefix=/media/black/Data/lib/crun/main
```

```shell
skopeo copy docker://alpine:latest oci:alpine:latest
umoci unpack --image alpine:latest alpine-bundle
chown abc:abc alpine-bundle
cd alpine-bundle

crun run sh
/ # ls
bin    etc    lib    mnt    proc   run    srv    tmp    var
dev    home   media  opt    root   sbin   sys    usr
```
