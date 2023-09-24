# iio

iio 子系统
![[imgs/Pasted image 20230923165947.png]]

[细节](https://www.eet-china.com/mp/a23369.html)

## 用户层

github libiio

它封装了对 /sys/bus/iio/devices(配置 iio) 和　/dev/iio/deviceX(读写iio) 的访问
并且提供了便于测试的 iio 命令行工具 (iio_info / iio_readdev) 和 iiod server。

![[imgs/Pasted image 20230923170654.png]]
