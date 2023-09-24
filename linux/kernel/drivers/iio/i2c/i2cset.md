# i2cset


:/ # i2cset                                                              
Usage: i2cset [-f] [-y] [-m MASK] [-r] I2CBUS CHIP-ADDRESS DATA-ADDRESS [VALUE] ... [MODE]
  I2CBUS is an integer or an I2C bus name
  ADDRESS is an integer (0x03 - 0x77)
  MODE is one of:
    c (byte, no value)
    b (byte data, default)
    w (word data)
    i (I2C block data)
    s (SMBus block data)
    Append p for SMBus PEC

```shell
:/ # i2cset -f -y 3 0x43 0x03 0x7f                                     
:/ # 
:/ # i2cget -f -y 3 0x43 0x03                                            
0x7f
```
