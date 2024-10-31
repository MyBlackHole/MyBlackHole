# crun

```shell
git clone git@github.com:containers/crun.git
cd crun
./autogen.sh
./configure CFLAGS='-g -O0' # --prefix=/run/media/black/Data/lib/crun/main
```

```shell
paru -S skopeo umoci

skopeo copy docker://alpine:latest oci:alpine:latest
❯ ls -TL 4
 .
└──  alpine
    ├──  blobs
    │   └──  sha256
    │       ├──  43c4264eed91be63b206e17d93e75256a6097070ce643c5e8f0379998b44f170
    │       ├──  c785b14aefb4a06a2362918e5fb06788b66a24e2c9ce7f0ef66042a006def4c7
    │       └──  d5dccdd0a13fa7ddf956718c41b003bd081b41765fd8c8b3cafbe585815dd4c2
    ├──  index.json
    └──  oci-layout

sudo umoci unpack --image alpine:latest alpine-bundle
sudo chown black:black alpine-bundle
cd alpine-bundle

crun run sh
/ # ls
bin    etc    lib    mnt    proc   run    srv    tmp    var
dev    home   media  opt    root   sbin   sys    usr
```

```shell
crun run --bundle alpine-bundle sh
/ # ls
bin    etc    lib    mnt    proc   run    srv    tmp    var
dev    home   media  opt    root   sbin   sys    usr
```
