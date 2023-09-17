# cvi_pinmux

引脚管理

```shell
[root@milkv]~# cvi_pinmux
cvi_pinmux for cv180x
./cvi_pinmux -p          <== List all pins
./cvi_pinmux -l          <== List all pins and its func
./cvi_pinmux -r pin      <== Get func from pin
./cvi_pinmux -w pin/func <== Set func to pin
```

- 所有 io 信息
```shell
cvi_pinmux -l
```

- 检查复用状态
```shell
[root@milkv]~# cvi_pinmux -r IIC0_SCL
IIC0_SCL function:
[ ] JTAG_TDI
[ ] UART1_TX
[ ] UART2_TX
[ ] XGPIOA_28
[v] IIC0_SCL
[ ] WG0_D0
[ ] DBG_10
```

- 修改引脚功能
```shell
cvi_pinmux -w IIC0_SCL/UART1_TX
```
