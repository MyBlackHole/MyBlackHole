# kmem

查看内核内存使用情况
```shell
kmem -i： 查看内存整体使用情况
kmem -s： 查看slab使用情况
kmem [addr]： 搜索地址所属的内存结构
```

```shell
crash> kmem -i
                 PAGES        TOTAL      PERCENTAGE
    TOTAL MEM  3968628      15.1 GB         ----
         FREE  2768319      10.6 GB   69% of TOTAL MEM
         USED  1200309       4.6 GB   30% of TOTAL MEM
       SHARED  1034992       3.9 GB   26% of TOTAL MEM
      BUFFERS    21049      82.2 MB    0% of TOTAL MEM
       CACHED  1058936         4 GB   26% of TOTAL MEM
         SLAB    69382       271 MB    1% of TOTAL MEM

   TOTAL HUGE        0            0         ----
    HUGE FREE        0            0    0% of TOTAL HUGE

   TOTAL SWAP        0            0         ----
    SWAP USED        0            0    0% of TOTAL SWAP
    SWAP FREE        0            0    0% of TOTAL SWAP

 COMMIT LIMIT  3571765      13.6 GB         ----
    COMMITTED    69883       273 MB    1% of TOTAL LIMIT
```
