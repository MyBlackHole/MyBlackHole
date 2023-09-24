# i2cget

:/ #  i2cget                                                              
Usage: i2cget [-f] [-y] I2CBUS CHIP-ADDRESS [DATA-ADDRESS [MODE]]
  I2CBUS is an integer or an I2C bus name
  ADDRESS is an integer (0x03 - 0x77)
  MODE is one of:
    b (read byte data, default)
    w (read word data)
    c (write byte/read byte)
    Append p for SMBus PEC

- 查看单个寄存器
```shell
以：总线 3 上 0x43 0x01 为例
:/ # i2cget -f -y 3 0x43 0x01                                          
0xa0
```
