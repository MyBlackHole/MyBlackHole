# ADB

- 启动
```shell
adb start-server
```

- 查看设备列表
```shell
adb devices
```

- 重启
```shell
adb reboot
```

- 杀死进程
```shell
adb shell kill -9 <pid>
```

- 重启应用
```shell
adb shell am start -n <package_name>/<activity_name>
```

- 截屏
```shell
adb shell screencap -p /sdcard/screenshot.png
adb pull /sdcard/screenshot.png
```


- 连接
```shell
adb connect 127.0.0.1:5555
adb connect 127.0.0.1
```

- 断开
```shell
adb disconnect 127.0.0.1:5555
adb disconnect 127.0.0.1
```

- 日志
```shell
adb logcat
```

- 查看已连接的设备
```shell
adb devices
```

- 查看 app 列表
```shell
adb shell pm list packages
```

- 卸载 app
```shell
adb uninstall <package_name>
adb uninstall com.example.app
```
