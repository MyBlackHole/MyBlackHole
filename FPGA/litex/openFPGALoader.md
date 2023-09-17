# openFPGALoader

Board name	FPGA	Memory	Flash
tangnano	GW1N-1 QN48	OK	Internal Flash
tangnano1k	GW1NZ-1 QN48	OK	Internal Flash
tangnano4k	GW1NSR-4C QN48	OK	Internal Flash
tangnano9k	GW1NR-9C QN88P	OK	Internal Flash
tangnano20k	GW2AR-18C QN88	OK	External Flash
tangprimer20k	GW2A-18C BGA256	OK	External Flash

烧录工具

- 检测
```shell
openFPGALoader --detect
No cable or board specified: using direct ft2232 interface
Jtag frequency : requested 6.00MHz   -> real 6.00MHz
index 0:
        idcode 0x81b
        manufacturer Gowin
        family GW2A
        model  GW2A(R)-18(C)
        irlength 8
```

- 下载码流
```shell
openFPGALoader -b tangnano9k -f ../../nano9k_lcd/impl/pnr/Tang_nano_9K_LCD.fs
# 其中的 -b 表示目标板型号，具体可以参考下面表格
# -f 表示下载到 flash，不加的话会下载到 sram 中
# 最后的是需要烧录的文件，应该找到对应目录下的 .fs 文件
write to flash
Jtag frequency : requested 6.00MHz   -> real 6.00MHz  
Parse file Parse ../../nano9k_lcd/impl/pnr/Tang_nano_9K_LCD.fs: 
Done
DONE
Jtag frequency : requested 2.50MHz   -> real 2.00MHz  
erase SRAM Done
erase Flash Done
write Flash: [==================================================] 100.00%
Done
CRC check: Success

```
