# error

- error: module/mpd: Connection refused
```shell
sudo apt install mpd
```

- 声音控制异常
```shell
aplay -l
**** PLAYBACK 硬體裝置清單 ****
card 0: Generic [HD-Audio Generic], device 3: HDMI 0 [HDMI 0]
  子设备: 1/1
  子设备 #0: subdevice #0
card 0: Generic [HD-Audio Generic], device 7: HDMI 1 [HDMI 1]
  子设备: 1/1
  子设备 #0: subdevice #0
card 1: Generic_1 [HD-Audio Generic], device 0: ALC257 Analog [ALC257 Analog]
  子设备: 0/1
  子设备 #0: subdevice #0

lvim ~/.asoundrc
defaults.pcm.card 1
defaults.pcm.device 0
defaults.ctl.card 1
```

- 亮度调节
```shell
git clone git@github.com:snobb/xbright.git
make 
sudo make install

xbright [+-=[0-N]]

-rwsr-xr-x 1 root root 21K Jan 13 20:12 /usr/local/bin/xbright
```
