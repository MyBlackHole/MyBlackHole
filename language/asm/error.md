# error

- start.S:11: Error: unrecognized opcode `csrr t0,mhartid', extension `zicsr' required
```shell
riscv64-unknown-elf-gcc -nostdlib -fno-builtin -march=rv32i_zicsr -march=rv32ima -mabi=ilp32 -g -Wall -c -o start.o start.S

rv32ima -> rv32g

riscv64-unknown-elf-gcc -nostdlib -fno-builtin -march=rv32i_zicsr -march=rv32g -mabi=ilp32 -g -Wall -c -o start.o start.S
```

- undefined reference to `__umodsi3`、undefined reference to `__udivdi3`
```shell
移除 -nostdlib
```

