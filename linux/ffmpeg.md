# ffmpeg
多媒体框架，能够解码、编码、 转码、复用、解复用、流、过滤和播放
## install
```shell
sudo apt-get install ffmpeg
```

## 例子

- 启动录制
```shell
ffmpeg -video_size 1920x1080 -framerate 25 -f x11grab -i :0.0 salida.mp4
# Ctrl + C 退出录制
```
**1920 × 1080** 录音的大小。
**帧率** 是每分钟的帧数。
**0.0** 是您要记录的区域。 您可以给X和Y起点以加号后的一部分记录在屏幕上，这看起来像 _0.0 100,200 +_ 对于从点X = 100和点Y = 200开始的窗口。
**output.mp4** 是输出文件。 如果我们按照上一条命令中的说明进行操作，则该文件将以“ output.mp4”的名称保存在我们的个人文件夹中

- 脉冲音频
```shell
ffmpeg -video_size 1920x1080 -framerate 25 -f x11grab -i :0.0 -f pulse -ac 2 -i default salida.mkv
```
