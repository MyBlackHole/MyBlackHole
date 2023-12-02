# zdb

```shell
root@black:/sandisk# zdb -l /dev/sdb1
------------------------------------
LABEL 0
------------------------------------
    version: 5000
    name: 'sandisk'
    state: 0
    txg: 1002
    pool_guid: 1620330189465436878
    errata: 0
    hostid: 104614418
    hostname: 'black'
    top_guid: 10088765877944140567
    guid: 10088765877944140567
    vdev_children: 1
    vdev_tree:
        type: 'disk'
        id: 0
        guid: 10088765877944140567
        path: '/dev/sdb1'
        devid: 'usb-SanDisk_Ultra_USB_3.0_05011df1ba7444a90afc6bce96bae0de9913183e4bfc5b32415f47f31f24b95843cf000000000000000000008087267a00101'
        phys_path: 'pci-0000:03:00.3-usb-0:2:1.0-scsi-0:0:0:0'
        whole_disk: 1
        metaslab_array: 256
        metaslab_shift: 29
        ashift: 9
        asize: 30750015488
        is_log: 0
        create_txg: 4
    features_for_read:
        com.delphix:hole_birth
        com.delphix:embedded_data
    labels = 0 1 2 3
```
