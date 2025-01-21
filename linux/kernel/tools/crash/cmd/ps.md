# ps

查看进程或线程状态

```shell
ps -p [pid]： 显示进程父子关系
ps -t [pid]： 显示进程运行时间
```

- 查看等待状态(D)的进程
```shell
crash> ps | grep UN
      707       2  37  ffff953c59b8af00  UN   0.0        0        0  [khugepaged]
   467332       1   6  ffff95bbe0835e00  UN   0.0  1480820    33188  agent70
  2332266 2331401  79  ffff95747aad0000  UN   0.0  1880700   141968  engine
  2332269 2331401   6  ffff958bcaf1af00  UN   0.0  1880700   141968  engine
  2332286 2331401   6  ffff958bcaf1c680  UN   0.0  1880700   141968  easymonitor_mai
  2332287 2331401  19  ffff953c5b03de00  UN   0.0  1880700   141968  easymonitor_mai
  2332289 2331401   2  ffff953c5b039780  UN   0.0  1880700   141968  easymonitor_mai
  2332291 2331401  80  ffff955653f1af00  UN   0.0  1880700   141968  easymonitor_mai
  2332492 2331401   6  ffff958d0543af00  UN   0.0  1880700   141968  notify-rs inoti
  2332627 2331401  68  ffff953c58b58000  UN   0.0  1880700   141968  engine
  2332635 2331401   3  ffff95bc2775c680  UN   0.0  1880700   141968  engine
  2332658 2331401  69  ffff953c58a59780  UN   0.0  1880700   141968  engine
```
