# asciinema
终端录制工具

## 安装
```shell
sudo apt install asciinema
```

## 命令
- 录制
```shell
asciinema rec test.cast
```

- 重放
```shell
asciinema play test.cast
# 重放远程
asciinema play url
```

- 打印记录到终端
```shell
asciinema cat test.cast
```

- 上传
```shell
asciinema upload demo.cast
```

- cast to gif
```shell
# 安装
npm install --global asciicast2gif
## 或
docker pull janlo/asciicast2gif
docker run --rm -v $PWD:/data janlo/asciicast2gif [options and arguments...]

# 转换
docker run --rm -v $PWD:/data janlo/asciicast2gif -s 2 -t solarized-dark test.cast test.gif
-t <theme>        color theme, one of: asciinema, tango, solarized-dark, solarized-light, monokai (default: asciinema)
-s <speed>        animation speed (default: 1)
-S <scale>        image scale / pixel density (default: 2)
-w <columns>      clip terminal to specified number of columns (width)
-h <rows>         clip terminal to specified number of rows (height)
```
