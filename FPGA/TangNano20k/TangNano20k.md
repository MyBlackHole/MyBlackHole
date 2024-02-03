# TangNano20k

|   |   |
|---|---|
|逻辑单元(LUT4)|20736|
|寄存器(FF)|15552|
|分布式静态随机存储器S-SRAM(bits)|41472|
|块状静态随机存储器B-SRAM(bits)|828K|
|块状静态随机存储器数目B-SRAM(个)|46|
|32bits SDR SDRAM|64M bits|
|乘法器(18x18 Multiplier)|48|
|锁相环(PLLs)|2|
|I/O Bank 总数|8|

|项目|参数|补充|
|---|---|---|
|FPGA 芯片|[GW2AR-LV18QN88C8/I7](http://www.gowinsemi.com.cn/prod_view.aspx?TypeId=10&FId=t3:10:3&Id=167#GW2AR)||
|板载下载器|BL616|· 给 FPGA 提供 JTAG 下载功能提供 USB 转串口与 FPGA 通信提供虚拟串口用于 FPGA 通过 SPI 通信提供虚拟串口控制 MS5351 输出时钟|
|时钟芯片|MS5351|给 FPGA 芯片提供额外的三路时钟[点我查看 MS5351 配置方法](https://wiki.sipeed.com/hardware/zh/tang/tang-nano-20k/example/unbox.html#pll_clk)|
|显示接口|· 40Pins RGB lcd 连接器 HDMI 接口||
|单色 LED|6 个|共阳极连接|
|RGB LED|1 个|型号是 WS2812|
|用户按键|2 个|用于自定义逻辑功能|
|TF 卡槽|1 个|推拉式|
|功率放大器|1 个|型号是 MAX98357A，用于播放音频|
|存储|64Mbits Flash|下载方式参考底部相关问题|
|尺寸|22.55mm x 54.04mm|精确尺寸可以参考 3D 文件|

![[imgs/Pasted image 20240203090308.png]]
