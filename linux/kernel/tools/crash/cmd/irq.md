# irq

查看中断号和中断资源的对应关系

```shell
crash> irq
 IRQ   IRQ_DESC/_DATA      IRQACTION      NAME
  0   ffff953c40024800  ffffffffb9a202c0  "timer"
  1   ffff953c40024a00      (unused)
  2   ffff953c40024c00      (unused)
  3   ffff953c40024e00      (unused)
  4   ffff953c40025000      (unused)
  5   ffff953c40025200      (unused)
  6   ffff953c40025400      (unused)
  7   ffff953c40025600  ffff956bc27bd100  "pinctrl_amd"
  8   ffff953c40025800  ffff959bc0d69000  "rtc0"
  9   ffff953c40025a00  ffff956bc0a03280  "acpi"
 10   ffff953c40025c00      (unused)
 11   ffff953c40025e00      (unused)
 12   ffff953c40026000      (unused)
 13   ffff953c40026200      (unused)
 14   ffff953c40026400      (unused)
 15   ffff953c40026600      (unused)
 16       (unused)          (unused)
 17       (unused)          (unused)
 18       (unused)          (unused)
 19       (unused)          (unused)
 20       (unused)          (unused)
 21       (unused)          (unused)
 22       (unused)          (unused)
 23       (unused)          (unused)
 24   ffff953c59c6fe00      (unused)
 25   ffff955bc0359600      (unused)
 26   ffff959bc1ae1a00      (unused)
 27   ffff95bbc0352e00      (unused)
 28   ffff953c5b01f000  ffff95cbc20e5400  "AMD-Vi"
 29   ffff955bc035b600  ffff95cbc20e5700  "AMD-Vi"
 30   ffff956bc1e43e00  ffff95cbc20e4e00  "AMD-Vi"
 31   ffff958bc0364e00  ffff95cbc20e4d80  "AMD-Vi"
 32   ffff959bc2e69800  ffff95cbc20e4080  "AMD-Vi"
 33   ffff95bbc1f26c00  ffff95cbc20e4880  "AMD-Vi"
 34   ffff95cbc03bb600  ffff95cbc20e5e00  "AMD-Vi"
 35   ffff95ebc1f49600  ffff95cbc20e5180  "AMD-Vi"
 36   ffff953c5b01b000  ffff953c4002bb00  "PCIe PME"
 37   ffff953c5b01e600  ffff953c4002bb80  "PCIe PME"
 38   ffff953c5b01a200  ffff953c4002bc00  "PCIe PME"
 39   ffff953c5b018200      (unused)
 40   ffff953c5b01f400  ffff953c4002bc80  "PCIe PME"
 41   ffff955bc035b400  ffff955bc0a80200  "PCIe PME"
 42   ffff955bc0358e00  ffff955bc0a81d80  "PCIe PME"
 43   ffff955bc0358800      (unused)
 44   ffff955bc035ec00  ffff955bc0a81a00  "PCIe PME"
 45   ffff956bc037dc00  ffff956bc1e3e700  "PCIe PME"
 46   ffff956bc037f800      (unused)
 47   ffff956bc037f600  ffff956bc1e3e100  "PCIe PME"
 48   ffff956bc037d400      (unused)
 49   ffff956bc0379200  ffff956bc1e3f380  "PCIe PME"
 50   ffff958bc0365200  ffff958bc0aa2e80  "PCIe PME"
                        ffff958bc0aa2680  "pciehp"
 51   ffff958bc0367200      (unused)
 52   ffff958bc0364800  ffff958bc0aa3e80  "PCIe PME"
```
