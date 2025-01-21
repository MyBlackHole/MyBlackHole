# sym

查看符号列表信息

```shell
crash> sym usb
symbol not found: usb
possible alternatives:
  ffffffffb8e24640 (T) usb_disabled
  ffffffffb8e24700 (T) usb_find_common_endpoints
  ffffffffb8e247c0 (T) usb_find_common_endpoints_reverse
  ffffffffb8e248a0 (T) usb_ifnum_to_if
  ffffffffb8e24900 (T) usb_altnum_to_altsetting
  ffffffffb8e24970 (t) usb_dev_prepare
  ffffffffb8e24980 (T) __usb_get_extra_descriptor
  ffffffffb8e249f0 (T) usb_find_interface
  ffffffffb8e24a70 (T) usb_put_dev
  ffffffffb8e24a90 (T) usb_put_intf
  ffffffffb8e24ab0 (T) usb_for_each_dev
  ffffffffb8e24b10 (t) usb_dev_restore
  ffffffffb8e24b20 (t) usb_dev_thaw
  ffffffffb8e24b30 (t) usb_dev_resume
  ffffffffb8e24b40 (t) usb_dev_poweroff
  ffffffffb8e24b50 (t) usb_dev_freeze
  ffffffffb8e24b60 (t) usb_dev_suspend
  ffffffffb8e24b70 (t) usb_dev_complete
  ffffffffb8e24b80 (t) usb_release_dev
  ffffffffb8e24be0 (t) usb_devnode
  ffffffffb8e24c10 (t) usb_dev_uevent
  ffffffffb8e24c60 (T) usb_get_dev
```
