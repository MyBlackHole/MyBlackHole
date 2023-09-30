# rtsp

- 拉流 
```shell
ffplay rtsp://127.0.0.1:8554/stream 
ffmpeg -stimeout 30000000 -i rtsp://127.0.0.1:8554/stream -c copy output.mp4 
```

|  1 |  2 |
|---|---|
|udp|ffmpeg -re -i input.mp4 -c copy -f rtsp rtsp://127.0.0.1:8554/stream|
|tcp|ffmpeg -re -i input.mp4 -c copy -rtsp_transport tcp -f rtsp rtsp://127.0.0.1:8554/stream|
|循环推流|ffmpeg -re -stream_loop -1 -i input.mp4 -c copy -f rtsp rtsp://127.0.0.1:8554/stream|

-re 为以流的方式读取； 
-stream_loop 为循环读取视频源的次数，-1为无限循环； 
-i 为输入的文件； 
-f 为格式化输出到哪里
