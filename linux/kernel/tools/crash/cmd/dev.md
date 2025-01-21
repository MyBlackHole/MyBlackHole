# dev

查看设备的情况

```shell
crash> dev
CHRDEV    NAME                 CDEV        OPERATIONS
   1      mem            ffff956bc0a86180  memory_fops
   2      pty            ffff95cbc265a240  tty_fops
   3      ttyp           ffff95cbc2680600  tty_fops
   4      /dev/vc/0      ffffffffba53d6c0  console_fops
   4      tty            ffff959bc0ea9500  tty_fops
   4      ttyS           ffff955bc24e4480  tty_fops
   5      /dev/tty       ffffffffba53be60  tty_fops
   5      /dev/console   ffffffffba53bdc0  console_fops
   5      /dev/ptmx      ffffffffba53c020  ptmx_fops
   7      vcs            ffff959bc0ea92c0  vcs_fops
  10      misc           ffff953c59c56180  misc_fops
  13      input          ffff955bc24f2468  joydev_fops
  21      sg             ffff958bc20dce40  sg_fops
  29      fb             ffff959bc0dc5440  fb_fops
 128      ptm            ffff955bc24e5980  tty_fops
 136      pts            ffff955bc24e5140  tty_fops
 162      raw            ffffffffba5446c0  raw_fops
 180      usb            ffff953c59c56780  usb_fops
 188      ttyUSB              (none)
 189      usb_device     ffffffffba54ba80  usbdev_file_operations
 202      cpu/msr        ffff95bbc0b24fc0  msr_fops
 203      cpu/cpuid      ffff95bbc0b24cc0  cpuid_fops
 226      drm            ffff958bc212e480  drm_stub_fops
 236      aux            ffff953c5910c3c0  auxdev_fops
 237      ipmidev        ffff953c59851ec0  ipmi_fops
 238      uio            ffff958bc2321d40  uio_fops
 239      hidraw         ffffffffba5a2ca0  hidraw_ops
 240      ttyDBC              (none)
 241      ttyDBC              (none)
 242      ttyDBC              (none)
 243      ttyDBC              (none)
 244      usbmon         ffffffffba54bc40  mon_fops_binary
 245      bsg            ffffffffba535440  bsg_fops
 246      hmm_device          (none)
 247      watchdog            (none)
 248      ptp            ffff955bc4360050  posix_clock_file_operations
 249      pps                 (none)
 250      cec                 (none)
 251      rtc            ffff958bc09a8c00  rtc_dev_fops
 252      dax                 (none)
 253      tpm                 (none)
 254      gpiochip       ffff956bc2623bd0  gpio_fileops

BLKDEV    NAME                GENDISK      OPERATIONS
 259      blkext              (none)
   8      sd             ffff953c7ee75800  sd_fops
   9      md                  (none)
  65      sd             ffff9545bcd28800  sd_fops
  66      sd             ffff953c699f9800  sd_fops
  67      sd             ffff953c69944000  sd_fops
  68      sd             ffff953fa35b0000  sd_fops
  69      sd             ffff9542de9a4800  sd_fops
  70      sd             ffff953c65c6f800  sd_fops
  71      sd             ffff954f357ed800  sd_fops
 128      sd             ffff954f357ea800  sd_fops
 129      sd                  (none)
 130      sd                  (none)
 131      sd                  (none)
 132      sd                  (none)
 133      sd                  (none)
 134      sd                  (none)
 135      sd                  (none)
 230      zvol           ffff95f1d1887000  zvol_ops
 253      device-mapper  ffff95ebcedd9800  dm_blk_dops
 254      mdp                 (none)
```
