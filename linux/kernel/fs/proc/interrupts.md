# interrupts

中断计数
/proc/interrupts

```shell
cat /proc/interrupts
<!-- 中断计数 -->
            CPU0       CPU1       CPU2       CPU3       CPU4       CPU5       CPU6       CPU7       CPU8       CPU9       CPU10      CPU11      CPU12      CPU13      CPU14      CPU15
   0:         35          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-IO-APIC    2-edge      timer
   1:         19          0          0          0          0          0          0          0          0          0         10          0          0          0          0          0  IR-IO-APIC    1-edge      i8042
   4:    1065454          0          0    4034207          0          0          0          0          0          0          0          0          0          0          0          0  IR-IO-APIC    4-edge      AMDI0010:01
   6:    4602287          0          0          0    3848508          0          0          0          0          0          0          0          0          0          0          0  IR-IO-APIC    6-edge      AMDI0010:02
   7:     307100          0          0          0          0          0     294702          0          0          0          0          0          0          0          0          0  IR-IO-APIC    7-fasteoi   pinctrl_amd
   8:          0          0          0          0          0          0          0          0          0          0          0          1          0          0          0          0  IR-IO-APIC    8-edge      rtc0
   9:          1          3          0          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-IO-APIC    9-fasteoi   acpi
  10:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-IO-APIC   10-edge      AMDI0010:00
  25:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0  IOMMU-MSI    0-edge      AMD-Vi
  26:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSI-0000:00:02.2    0-edge      PCIe PME
  27:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSI-0000:00:02.4    0-edge      PCIe PME
  28:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSI-0000:00:08.1    0-edge      PCIe PME
  29:      10661          0          0          0          0          0      40391          0          0          0          0          0          0          0          0          0  amd_gpio   90  ITE8227:00
  30:     296446          0          0          0          0          0     254329          0          0          0          0          0          0          0          0          0  amd_gpio   89  MSFT0001:00
  32:       5892          0       2196        107      67228       1736      61168      25972      17107      62874      26868      67005     135686      15235      56910      43047  IR-PCI-MSIX-0000:03:00.3    0-edge      xhci_hcd
  33:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSIX-0000:03:00.3    1-edge      xhci_hcd
  34:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSIX-0000:03:00.3    2-edge      xhci_hcd
  35:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSIX-0000:03:00.3    3-edge      xhci_hcd
  36:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSIX-0000:03:00.3    4-edge      xhci_hcd
  37:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSIX-0000:03:00.3    5-edge      xhci_hcd
  38:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSIX-0000:03:00.3    6-edge      xhci_hcd
  39:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSIX-0000:03:00.3    7-edge      xhci_hcd
  41:         21          0          0          0        149          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSIX-0000:03:00.4    0-edge      xhci_hcd
  42:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSIX-0000:03:00.4    1-edge      xhci_hcd
  43:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSIX-0000:03:00.4    2-edge      xhci_hcd
  44:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSIX-0000:03:00.4    3-edge      xhci_hcd
  45:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSIX-0000:03:00.4    4-edge      xhci_hcd
  46:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSIX-0000:03:00.4    5-edge      xhci_hcd
  47:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSIX-0000:03:00.4    6-edge      xhci_hcd
  48:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSIX-0000:03:00.4    7-edge      xhci_hcd
  50:          0       3134          0          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSI-0000:03:00.6    0-edge      snd_hda_intel:card1
  51:          0          0         37          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSIX-0000:02:00.0    0-edge      nvme0q0
  52:       9637          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSIX-0000:02:00.0    1-edge      nvme0q1
  53:          0       5191          0          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSIX-0000:02:00.0    2-edge      nvme0q2
  54:          0          0       9484          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSIX-0000:02:00.0    3-edge      nvme0q3
  55:          0          0          0       5660          0          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSIX-0000:02:00.0    4-edge      nvme0q4
  56:          0          0          0          0       9151          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSIX-0000:02:00.0    5-edge      nvme0q5
  57:          0          0          0          0          0       7076          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSIX-0000:02:00.0    6-edge      nvme0q6
  58:          0          0          0          0          0          0      10118          0          0          0          0          0          0          0          0          0  IR-PCI-MSIX-0000:02:00.0    7-edge      nvme0q7
  59:          0          0          0          0          0          0          0       4852          0          0          0          0          0          0          0          0  IR-PCI-MSIX-0000:02:00.0    8-edge      nvme0q8
  60:          0          0          0          0          0          0          0          0       8959          0          0          0          0          0          0          0  IR-PCI-MSIX-0000:02:00.0    9-edge      nvme0q9
  61:          0          0          0          0          0          0          0          0          0       5658          0          0          0          0          0          0  IR-PCI-MSIX-0000:02:00.0   10-edge      nvme0q10
  62:          0          0          0          0          0          0          0          0          0          0       8377          0          0          0          0          0  IR-PCI-MSIX-0000:02:00.0   11-edge      nvme0q11
  63:          0          0          0          0          0          0          0          0          0          0          0       5334          0          0          0          0  IR-PCI-MSIX-0000:02:00.0   12-edge      nvme0q12
  64:          0          0          0          0          0          0          0          0          0          0          0          0       7807          0          0          0  IR-PCI-MSIX-0000:02:00.0   13-edge      nvme0q13
  65:          0          0          0          0          0          0          0          0          0          0          0          0          0       4753          0          0  IR-PCI-MSIX-0000:02:00.0   14-edge      nvme0q14
  66:          0          0          0          0          0          0          0          0          0          0          0          0          0          0       9321          0  IR-PCI-MSIX-0000:02:00.0   15-edge      nvme0q15
  67:       1687          0          0          0          0          0          0      46766          0       4703          0   10789342          0       2632          0    3813030  IR-PCI-MSIX-0000:03:00.0    0-edge      amdgpu
  69:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSIX-0000:03:00.2    0-edge      psp-1
  72:        440    3241837          0          0          0          0          0          0          0          0          0          0          0          0          0        559  IR-PCI-MSIX-0000:01:00.0    0-edge      iwlwifi:default_queue
  73:         96          0          0         41       1916       1081       1450      39418       1766       4833       2626       2972       5181       8557      15111       2044  IR-PCI-MSIX-0000:01:00.0    1-edge      iwlwifi:queue_1
  74:          2         19         12        118        688        188       3482       2954      54959       5939      60479       1519       8833     179213      40552       2391  IR-PCI-MSIX-0000:01:00.0    2-edge      iwlwifi:queue_2
  75:          3          0         26         34        658        586        612        954       1589      36821      10601      48953       6711       2076      40192       2159  IR-PCI-MSIX-0000:01:00.0    3-edge      iwlwifi:queue_3
  76:          3          0      12554         48        445        200      95076       2335      88139      59634     121461       3630      12817       7157     113563       2982  IR-PCI-MSIX-0000:01:00.0    4-edge      iwlwifi:queue_4
  77:         34          0          5         64      39866        673      30024      46898       2408       2330     266959       3786     164644       2674      12995       2146  IR-PCI-MSIX-0000:01:00.0    5-edge      iwlwifi:queue_5
  78:          4          0        151          0        800        641        842       8479       7149      95100      25929       1838      90315      71597      24875      74172  IR-PCI-MSIX-0000:01:00.0    6-edge      iwlwifi:queue_6
  79:         16          0          0        246        338        361      68279      78373       1102     117455     178704       2507      33835       1288        802       5338  IR-PCI-MSIX-0000:01:00.0    7-edge      iwlwifi:queue_7
  80:         11          0          0        289        133        477       1071       2263       2301       4691       1506       3264     122158       4672       4569      18154  IR-PCI-MSIX-0000:01:00.0    8-edge      iwlwifi:queue_8
  81:          3          0        723          0       1797       7048       1977       5572       4169       1528       1679       2083     138251     121943       1734       4561  IR-PCI-MSIX-0000:01:00.0    9-edge      iwlwifi:queue_9
  82:         54          0          0          1       1241       1566        814       1176       3069       2863       4508       1446       7369      91750       2053       1257  IR-PCI-MSIX-0000:01:00.0   10-edge      iwlwifi:queue_10
  83:         18          0          0         13        161        615      13310      68300     245569       5454     145332       2931     135799       5095      17086       6315  IR-PCI-MSIX-0000:01:00.0   11-edge      iwlwifi:queue_11
  84:          3          0          1         35       2078        305       1171       2774       2556       6689       7265       2414       1963      81382     152029      20204  IR-PCI-MSIX-0000:01:00.0   12-edge      iwlwifi:queue_12
  85:         29          0          0          0        150        748       9739       4111       4257       1869       5296     105083      11369       8721       2100       3727  IR-PCI-MSIX-0000:01:00.0   13-edge      iwlwifi:queue_13
  86:          1          0          0          0       2321      26946       1058       1131       6685       1329       3692       3715     259357      47186       2163       5103  IR-PCI-MSIX-0000:01:00.0   14-edge      iwlwifi:queue_14
  87:          1          0          0          0          0          1          0          0          0          0          0          0          0          0          3          0  IR-PCI-MSIX-0000:01:00.0   15-edge      iwlwifi:exception
  89:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0       6415  IR-PCI-MSIX-0000:02:00.0   16-edge      nvme0q16
  90:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0  IR-PCI-MSI-0000:03:00.1    0-edge      snd_hda_intel:card0
 NMI:        304        357        389        289        397        288        394        287        467        348        472        386        449        326        456        346   Non-maskable interrupts
 LOC:    7651921    5106190    7214436    4328414    7229060    4177738    7249227    4196271    9723380    6901665   10067853    7015352    9624525    6062337    9842174    6441842   Local timer interrupts
 SPU:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0   Spurious interrupts
 PMI:        304        357        389        289        397        288        394        287        467        348        472        386        449        326        456        346   Performance monitoring interrupts
 IWI:          0          0          1          0          0          0          3          4          0          9          2          1          1          1          3          3   IRQ work interrupts
 RTR:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0   APIC ICR read retries
 RES:     168539     128846     169423     116226     166887     115005     167212     115623     272276     240226     376124     209073     270878     214637     243929     186854   Rescheduling interrupts
 CAL:   11833735    6364229    7645258    5648640    7328543    5600462    7326981    5541415    9932104    8641616   11051047    8189045    9503014    7905677   10029922    8155890   Function call interrupts
 TLB:     338453     291720     382993     311896     373069     316608     371751     317788     292563     238671     286575     233124     298057     238776     289926     247085   TLB shootdowns
 TRM:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0   Thermal event interrupts
 THR:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0   Threshold APIC interrupts
 DFR:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0   Deferred Error APIC interrupts
 MCE:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0   Machine check exceptions
 MCP:        173        172        172        172        172        172        172        172        172        172        172        172        172        172        172        172   Machine check polls
 ERR:          0
 MIS:          0
 PIN:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0   Posted-interrupt notification event
 NPI:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0   Nested posted-interrupt event
 PIW:          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0          0   Posted-interrupt wakeup event
```

