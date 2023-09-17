# litex_term

```shell
usage: litex_term [-h] [--speed SPEED] [--serial-boot] [--kernel KERNEL]
                  [--kernel-adr KERNEL_ADR] [--images IMAGES] [--safe]
                  [--csr-csv CSR_CSV] [--base-address BASE_ADDRESS]
                  [--crossover-name CROSSOVER_NAME] [--jtag-name JTAG_NAME]
                  [--jtag-config JTAG_CONFIG] [--jtag-chain JTAG_CHAIN]
                  port

positional arguments:
  port                  Serial port (eg /dev/tty*, crossover, jtag).

optional arguments:
  -h, --help            show this help message and exit
  --speed SPEED         Serial baudrate. (default: 115200)
  --serial-boot         Automatically initiate serial boot. (default: False)
  --kernel KERNEL       Kernel image. (default: None)
  --kernel-adr KERNEL_ADR
                        Kernel address. (default: 0x40000000)
  --images IMAGES       JSON description of the images to load to memory.
                        (default: None)
  --safe                Safe serial boot mode, disable upload speed
                        optimizations. (default: False)
  --csr-csv CSR_CSV     SoC CSV file. (default: None)
  --base-address BASE_ADDRESS
                        CSR base address. (default: None)
  --crossover-name CROSSOVER_NAME
                        Crossover UART name to use (present in
                        design/csr.csv). (default: uart_xover)
  --jtag-name JTAG_NAME
                        JTAG UART type (jtag_uart). (default: jtag_uart)
  --jtag-config JTAG_CONFIG
                        OpenOCD JTAG configuration file for jtag_uart.
                        (default: openocd_xc7_ft2232.cfg)
  --jtag-chain JTAG_CHAIN
                        JTAG chain. (default: 1)
```

## 例子

- 连接串口
```shell
默认 125
litex_term /dev/ttyUSB1
```
