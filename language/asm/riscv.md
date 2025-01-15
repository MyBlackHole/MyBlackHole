# riscv

## install

```shell
paru -S riscv-gnu-toolchain-bin
```

## RV32I\RV32M\RV32A\RV32F\RV32D RV32G

| 指令集 | RV32I | RV32M | RV32A | RV32D | RV32F | RV32F与RV32D共同的指令 | RV32G |
| --- | ----- | ----- | ----- | ----- | ----- | ---------------- | ----- |
| 指令数 | 47    | 8     | 11    | 32    | 32    | 8                | 122   |

rv32g = rv32i + rv32m + rv32a + rv32f + rv32d - 8 = 47 + 8 + 11 + 32 + 32 - 8 = 122
