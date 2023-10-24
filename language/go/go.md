# 

## 安装
```shell
# 需要基础golang
go install golang.org/dl/go1.20@latest
go1.20 download

## 或

sudo add-apt-repository ppa:longsleep/golang-backports 
sudo apt install golang-go

## 或

rm -rf /usr/local/go && tar -C /usr/local -xzf go1.xx.x.linux-amd64.tar.gz 
export PATH=$PATH:/usr/local/go/bin 
```

## 配置
```shell
GOROOT : GO SDK 路径
GOPATH : GO go 项目路径

# 代理：
go env -w GO111MODULE=on  
go env -w GOPROXY=https://goproxy.cn,direct 
## 或 
export GO111MODULE=on 
export GOPROXY=https://goproxy.cn 

# 交叉编译
# gcc 
sudo apt install gcc libc6-dev 

# x11 
sudo apt install libx11-dev xorg-dev libxtst-dev 

# Bitmap 
sudo apt install libpng++-dev 

# Hook 
sudo apt install xcb libxcb-xkb-dev x11-xkb-utils libx11-xcb-dev libxkbcommon-x11-dev libxkbcommon-dev 

# Clipboard 
sudo apt install xsel xclip 
```

## 命令
```shell
升级golang到1.12以上 
GO111MODULE=auto 

随意项目内 
go mod init hello 
```

- 静态编译
```shell
CGO_ENABLED=0 go build
```

|                                |                                                 |
| ------------------------------ | ----------------------------------------------- |
| go run main.go                 | 运行go程序                                      |
| go build main.go               | 生成二进制文件                                  |
| go build -gcflags “-N -l”      | 可 gdb 调试二进制构建                           |
| go tool compile -N -l -S xx.go | 获取汇编代码                                    |
| go mod init mod(名称)          | 初始化go.mod                                    |
| go mod tidy                    | 整理依赖包                                      |
| go mod vendor                  | 缓存到vendor                                    |
| go env -w GOARCH=arm64         | 架构配置                                        |
| go env                         | 获取go环境变量                                  |
| Gdb 适配 goroutines            | source /usr/share/go/src/runtime/runtime-gdb.py |
