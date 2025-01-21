# mod

查看模块加载的情况

-s: 加载模块符号表
-d: 删除某个模块符号表
-S: 加载某个目录的模块符号表



```shell
# mod // 查看模块
# mod -s module /path/to/module.ko // 加载模块
# sym symbol // 显示符号对应的模块源码，也可以用virtual address


crash> mod
     MODULE       NAME                        BASE           SIZE  OBJECT FILE
ffffffffc01c9980  scsi_transport_iscsi  ffffffffc01b8000   110592  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc01d9180  dm_log                ffffffffc01d6000    20480  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc0212080  libiscsi              ffffffffc0207000    61440  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc021b080  libiscsi_tcp          ffffffffc0217000    24576  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc0222340  iscsi_tcp             ffffffffc021e000    24576  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc022b740  iscsi_boot_sysfs      ffffffffc0229000    16384  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc0273380  qla4xxx               ffffffffc0231000   303104  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc027f040  dm_region_hash        ffffffffc027c000    20480  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc028b480  libcxgb               ffffffffc0288000    20480  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc029c0c0  libcxgbi              ffffffffc0291000    61440  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc0305b40  cxgb4                 ffffffffc02a8000   430080  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc0318280  dm_multipath          ffffffffc0312000    32768  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc0320280  ip_tables             ffffffffc031b000    28672  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc032e440  cxgb4i                ffffffffc0325000    49152  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc033a3c0  uio                   ffffffffc0337000    20480  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc0350280  cnic                  ffffffffc0341000    73728  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc0366780  bnx2i                 ffffffffc0359000    65536  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc038ca00  be2iscsi              ffffffffc0370000   131072  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc03aeb40  dm_mod                ffffffffc0391000   155648  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc03bd280  dm_mirror             ffffffffc03b8000    28672  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc040de00  sunrpc                ffffffffc03c0000   446464  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc0433340  pinctrl_amd           ffffffffc042e000    28672  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc04404c0  sd_mod                ffffffffc0436000    53248  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc0446040  libcrc32c             ffffffffc0444000    16384  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc047a8c0  ngbe                  ffffffffc0455000   176128  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc04921c0  fat                   ffffffffc0481000    86016  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc04a1080  vfat                  ffffffffc049d000    24576  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc04b6040  nls_utf8              ffffffffc04b4000    16384  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc04f29c0  libata                ffffffffc04bf000   270336  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc059a440  i40e                  ffffffffc0517000   602112  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc05ce0c0  isofs                 ffffffffc05c5000    49152  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc05f17c0  nf_tables             ffffffffc05d4000   143360  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc05fa0c0  nft_counter           ffffffffc05f8000    16384  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc06364c0  rfkill                ffffffffc0632000    28672  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc063c040  unix_diag             ffffffffc063a000    16384  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc0653080  inet_diag             ffffffffc064f000    24576  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc065f040  udp_diag              ffffffffc065d000    16384  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc066e500  scsi_transport_sas    ffffffffc0667000    45056  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc067a580  libahci               ffffffffc0673000    40960  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc0691640  smartpqi              ffffffffc0682000    73728  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc0697000  sysimgblt             ffffffffc0695000    16384  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc06bfe40  bonding               ffffffffc069a000   188416  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc06cb000  tcp_diag              ffffffffc06c9000    16384  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc06d00c0  dm_service_time       ffffffffc06ce000    16384  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc06e2680  spl                   ffffffffc06d3000   106496  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc0700340  target_core_iblock    ffffffffc06fd000    20480  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc070b500  target_core_file      ffffffffc0707000    24576  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc0710000  zavl                  ffffffffc070e000    16384  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc071d080  znvpair               ffffffffc0713000    77824  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc072b7c0  target_core_pscsi     ffffffffc0727000    24576  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc073bc80  target_core_user      ffffffffc0732000    53248  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc0747080  grace                 ffffffffc0745000    16384  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc0762600  lockd                 ffffffffc074d000   110592  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc0773000  nfs_acl               ffffffffc0771000    16384  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc0786180  auth_rpcgss           ffffffffc0778000    73728  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc07da000  zunicode              ffffffffc078b000   331776  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc07f4900  ahci                  ffffffffc07ec000    40960  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc0800400  ghash_clmulni_intel   ffffffffc07fe000    16384  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc0858ac0  nfsd                  ffffffffc0803000   417792  (not loaded)  [CONFIG_KALLSYMS]
ffffffffc0875e80  zcommon               ffffffffc086a000    86016  (not loaded)  [CONFIG_KALLSYMS]

```
