# dis

- 反汇编指定的函数
```shell
crash_arm64_v8> dis hang_detect_thread
0xffffffc0109f85ac <hang_detect_thread>:        sub     sp, sp, #0xa0
0xffffffc0109f85b0 <hang_detect_thread+4>:      str     x30, [x18], #8
0xffffffc0109f85b4 <hang_detect_thread+8>:      stp     x29, x30, [sp, #64]
...
0xffffffc0109f85cc <hang_detect_thread+32>:     add     x29, sp, #0x40
0xffffffc0109f85d0 <hang_detect_thread+36>:     adrp    x8, 0xffffffc011e83000 <vsock_dgram_ops+144>
0xffffffc0109f85d4 <hang_detect_thread+40>:     ldr     x8, [x8, #1672]
```

- 反汇编指定地址
```shell
crash_arm64_v8> dis 0xffffffc0109f8610
0xffffffc0109f8610 <hang_detect_thread+100>:    mov     w3, #0x1                        // #1
crash_arm64_v8> dis 0xffffffc0109f8610 6
0xffffffc0109f8610 <hang_detect_thread+100>:    mov     w3, #0x1                        // #1
0xffffffc0109f8614 <hang_detect_thread+104>:    mov     x0, x19
0xffffffc0109f8618 <hang_detect_thread+108>:    stp     w8, w9, [sp, #16]
0xffffffc0109f861c <hang_detect_thread+112>:    bl      0xffffffc0101ec1dc <__sched_setscheduler>
0xffffffc0109f8620 <hang_detect_thread+116>:    adrp    x0, 0xffffffc0125bb000 <static_ltree+24>
0xffffffc0109f8624 <hang_detect_thread+120>:    add     x0, x0, #0xe58
```

- -l 参数: 显示反汇编和源码
```shell
crash_arm64_v8> dis -l hang_detect_thread
/home/bryan.he/sprdroid12_trunk_22b_ump518_bringup/bsp/kernel/kernel5.4/drivers/soc/sprd/debug/sysdump/native_hang_monitor.c: 599
0xffffffc0109f85ac <hang_detect_thread>:        sub     sp, sp, #0xa0
0xffffffc0109f85b0 <hang_detect_thread+4>:      str     x30, [x18], #8
....
0xffffffc0109f85cc <hang_detect_thread+32>:     add     x29, sp, #0x40
0xffffffc0109f85d0 <hang_detect_thread+36>:     adrp    x8, 0xffffffc011e83000 <vsock_dgram_ops+144>
...
0xffffffc0109f85dc <hang_detect_thread+48>:     nop
0xffffffc0109f85e0 <hang_detect_thread+52>:     mov     w8, #0x1                        // #1
/home/bryan.he/sprdroid12_trunk_22b_ump518_bringup/bsp/kernel/kernel5.4/kernel/sched/core.c: 5428
0xffffffc0109f85e4 <hang_detect_thread+56>:     stp     xzr, xzr, [sp, #40]
...
```

- -s参数: 显示该函数的文件名和源代码
```shell
crash_arm64_v8> dis -s hang_detect_thread
FILE: /home/bryan.he/sprdroid12_trunk_22b_ump518_bringup/bsp/kernel/kernel5.4/drivers/soc/sprd/debug/sysdump/native_hang_monitor.c
LINE: 599

dis: hang_detect_thread: source code is not available

```


