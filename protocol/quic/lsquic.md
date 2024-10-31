# lsquic

## source code
```shell
git clone https://github.com/litespeedtech/lsquic.git
```


## example

- echo
```shell
./bin/echo_server \
    -s 127.0.0.1:3345 \
    -l event=info,engine=debug \
    -c wdg,/run/media/black/Data/Documents/c/lsquic_learn/demo2/certs/server.cert,/run/media/black/Data/Documents/c/lsquic_learn/demo2/certs/server.key

./bin/echo_client \
    -s 127.0.0.1:3345 \
    -l event=debug,engine=debug \
    -L debug

./bin/echo_client \
    -s 127.0.0.1:3345 \
    -l event=info,engine=debug
```

- perf
```shell
./bin/perf_server \
    -s 127.0.0.1:3345 \
    -l event=info,engine=debug \
    -c wdg,/run/media/black/Data/Documents/c/lsquic_learn/demo2/certs/server.cert,/run/media/black/Data/Documents/c/lsquic_learn/demo2/certs/server.key

./bin/perf_client \
    -s 127.0.0.1:3345 \
    -l event=info,engine=debug \
    -p 1:1
```

- md5
```shell
./bin/md5_server \
    -s 127.0.0.1:3345 \
    -l event=info,engine=debug \
    -c wdg,/run/media/black/Data/Documents/c/lsquic_learn/demo2/certs/server.cert,/run/media/black/Data/Documents/c/lsquic_learn/demo2/certs/server.key

./bin/md5_client \
    -s 127.0.0.1:3345 \
    -l event=info,engine=debug \
    -f ./CMakeCache.txt
```

- duck
```shell
./bin/duck_server \
    -s 127.0.0.1:3345 \
    -l event=info,engine=debug \
    -c wdg,/run/media/black/Data/Documents/c/lsquic_learn/demo2/certs/server.cert,/run/media/black/Data/Documents/c/lsquic_learn/demo2/certs/server.key

./bin/duck_client \
    -s 127.0.0.1:3345 \
    -l event=info,engine=debug
```
