# build
构建

```shell
sudo apt-get install git wget rpm rpm2cpio cpio make build-essential binutils m4

bash build.sh debug --init --make (或许失败)

<!-- 失败就 -->
cd build_debug

make observer
```

```shell
# deploy an instance of the largest size according to the current container
docker run -p 2881:2881 --name obstandalone -d oceanbase/oceanbase-ce

# deploy mini standalone instance
docker run -p 2881:2881 --name obstandalone -e MINI_MODE=1 -d oceanbase/oceanbase-ce
```
