- spi_device register error /spi2@041A0000/spi-flash@0
- Failed to create SPI device for /spi2@041A0000/spi-flash@0
```shell
drivers/spi/spi.c
of_register_spi_devices
of_register_spi_device
spi_add_device
```
