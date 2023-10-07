# boot

引导过程：
![[imgs/Pasted image 20231003172322.png]]


[详细](https://community.milkv.io/t/uboot/181)

## 引导自己的内核

bootm 是引导 linux kernel 的
包含了引导协议的一些东西
我们作为一个裸机程序
我们可以使用 uboot 的 go 命令直接跳转
我们需要在 uboot 的 defconfig 中打开 go 命令的配置
```shell
diff --git a/boards/cv180x/cv1800b_milkv_duo_sd/u-boot/cvitek_cv1800b_milkv_duo_sd_defconfig b/boards/cv180x/cv1800b_milkv_duo_sd/u-boot/cvitek_cv1800b_milkv_duo_sd_defconfig
index 1bb7fde..e821fcb 100644
--- a/boards/cv180x/cv1800b_milkv_duo_sd/u-boot/cvitek_cv1800b_milkv_duo_sd_defconfig
+++ b/boards/cv180x/cv1800b_milkv_duo_sd/u-boot/cvitek_cv1800b_milkv_duo_sd_defconfig
@@ -32,7 +32,7 @@ CONFIG_BOOTM_OPENRTOS=y
 # CONFIG_BOOTM_VXWORKS is not set
 # CONFIG_CMD_ELF is not set
 # CONFIG_CMD_FDT is not set
-# CONFIG_CMD_GO is not set
+CONFIG_CMD_GO=y
 # CONFIG_CMD_IMI is not set
 # CONFIG_CMD_XIMG is not set
 # CONFIG_CMD_EXPORTENV is not set

diff --git a/boards/cv180x/cv1800b_milkv_duo_sd/u-boot/cvitek_cv1800b_sophpi_duo_sd_rls_defconfig b/boards/cv180x/cv1800b_milkv_duo_sd/u-boot/cvitek_cv1800b_sophpi_duo_sd_rls_defconfig
index b4250f2..4419c0c 100644
--- a/boards/cv180x/cv1800b_milkv_duo_sd/u-boot/cvitek_cv1800b_sophpi_duo_sd_rls_defconfig
+++ b/boards/cv180x/cv1800b_milkv_duo_sd/u-boot/cvitek_cv1800b_sophpi_duo_sd_rls_defconfig
@@ -32,7 +32,7 @@ CONFIG_BOOTM_OPENRTOS=y
# CONFIG_BOOTM_VXWORKS is not set
# CONFIG_CMD_ELF is not set
# CONFIG_CMD_FDT is not set
-# CONFIG_CMD_GO is not set
+CONFIG_CMD_GO=y
# CONFIG_CMD_IMI is not set
# CONFIG_CMD_XIMG is not set
# CONFIG_CMD_EXPORTENV is not set
```

- 重新编译 fip.bin
```shell
source build/cvisetup.sh
defconfig cv1800b_milkv_duo_sd
clean_all
build_fsbl

<!-- 结果文件路径 -->
duo-buildroot-sdk/install/soc_cv1800b_milkv_duo_sd/fip.bin
```

- 修改引导自己内核
```shell
u-boot-2021.10/include/configs/cv180x-asic.h

diff --git a/include/configs/cv180x-asic.h b/include/configs/cv180x-asic.h
index 08b70193..f1cfc95e 100644
--- a/include/configs/cv180x-asic.h
+++ b/include/configs/cv180x-asic.h
@@ -283,12 +283,9 @@
                    "console=$consoledev,$baudrate $othbootargs;"

    #define SD_BOOTM_COMMAND \
-               SET_BOOTARGS \
                "echo Boot from SD ...;" \
-               "mmc dev 0 && fatload mmc 0 ${uImage_addr} boot.sd; " \
-               "if test $? -eq 0; then " \
-               UBOOT_VBOOT_BOOTM_COMMAND \
-               "fi;"
+               "mmc dev 0 && fatload mmc 0 ${uImage_addr} kernel.bin; " \
+               "go ${uImage_addr}"

    #ifndef CONFIG_SD_BOOT
        #define CONFIG_BOOTCOMMAND  SHOWLOGOCMD "cvi_update || run norboot || run nandboot ||run emmcboot"
```

- dts 修改
```shell
udisksctl mount -b /dev/sdb1
cd /media/black/boot
cp /media/black/Data/Documents/github/Demo/Milkv/duo-buildroot-sdk/install/soc_cv1800b_milkv_duo_sd/rawimages/boot.sd .
cp /media/black/Data/Documents/github/Demo/Milkv/duo-buildroot-sdk/install/soc_cv1800b_milkv_duo_sd/fip.bin .
```
