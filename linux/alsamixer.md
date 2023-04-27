声音控制

## 配置
```ini
/etc/asound.conf 

defaults.pcm.card 1 

defaults.pcm.device 0 

defaults.ctl.card 1 

“pcm”选项决定用来播放音频的设备，而“ctl”选项决定那个声卡能够由控制工具（如 alsamixer）使用
```