# error

- ERROR: modpost: "kallsyms_lookup_name" [my_syscall.ko] undefined!
```shell
CONFIG_KALLSYMS = y
CONFIG_KALLSYMS_ALL = y 
CONFIG_KALLSYMS_ABSOLUTE_PERCPU  = y
CONFIG_KALLSYMS_BASE_RELATIVE = y
```


- error:  /soc/qcom,pcie@1c00000: could not get #gpio-cells for /soc/qcom,rimps@17C00000
解析：
    1. #gpio-cells    属性是一个GPIO controller的必须定义的属性，它描述了需要多少个cell来具体描述一个GPIO（这是和具体的GPIO controller相关的）。
    2. cell表示一个无符号的32位整数，xxxx-cells指定用多少个cell描述xxxx属性
    3. “#gpio-cells = <2>”表示这个控制器下每一个引脚要用2个32位的数(cell)来描述。
    4. 为什么要用2个数？其实使用多个cell来描述一个引脚，这是GPIO Controller自己决定的。比如可以用其中一个cell来表示那是哪一个引脚，用另一个cell来表示它是高电平有效还是低电平有效，甚至还可以用更多的cell来示其他特性。
        普遍的用法是，用第1个cell来表示哪一个引脚，用第2个cell来表示有效电平：
        GPIO_ACTIVE_HIGH ：高电平有效
        GPIO_ACTIVE_LOW : 低电平有效
        修改文件为pinctrol的设备分单和多的写法： pinctrl(1)——pinctrl子系统的使用 - 爱码网


- error:  rpmh_regulator_probe: ldoe2: could not find RPMh address for resource
```shell

解析： X1 代码使用rpmh代码非高通文件--》流程使用rpmh-regulator.c， 开启宏：CONFIG_REGULATOR_RPMH
       而不是使用qcom-rpmh-regulator.c， 项目没有开启宏： CONFIG_REGULATOR_QCOM_RPMH

代码部分： 
    /kernel/msm-5.4/drivers/regulator/rpmh-regulator.c
    aggr_vreg->addr = cmd_db_read_addr(aggr_vreg->resource_name);
     if (!aggr_vreg->addr) { 
     aggr_vreg_err(aggr_vreg, "could not find RPMh address for resource\n"); 
     return -ENODEV;
    }
    确认修改点在device tree 部分，代码中获取不到对应的设备树地址。代码打打印node 成员name 和 resource_name， 确认
    rpmh-regulator-ldoe2， rpmh-regulator-ldoe3， rpmh-regulator-ldoe4， rpmh-regulator-ldoe5， rpmh-regulator-ldoe6
    无法匹配上并且初始化是处于失败状态。确认共性，上述的节点都是pmr735a相关的，X1项目是wifi only， 没有这个pm芯片。
    所以修改点应该是项目独立去除这些node。
```


- qcom,qpnp-power-on c440000.qcom,spmi: qcom,pmk8350@0:pon_hlos@1300: IRQ pmic-wd-bark not found
```shell
qcom,qpnp-power-on c440000.qcom,spmi: qcom,pm7250b@2:qcom,power-on@800: IRQ pmic-wd-bark not found
修改方法：报错是因为属性值pmic-wd-bark 没有添加，参看平台和公司几个已有项目都是做不添加处理，修改方案新增项目属性值不做 
pmic-wd-bark 获取。
“pmic-wd-bark" interrupt can be added if the system needs to handle PMIC watchdog barks.
```


- scm_mem_protection_init_do: SCM call failed
```shell
函数关系： kernel/msm-5.4/drivers/firmware/qcom_scm.c （early_initcall(scm_mem_protection_init)）
→ kernel/msm-5.4/drivers/firmware/qcom_scm-smc.c (scm_mem_protection_init_do)
→ kernel/msm-5.4/drivers/firmware/qcom_scm-smc.c (qcom_scm_call->qcom_scm_call_smccc)
```


- energy_model: Power domains created prior to em_debug_init
```shell
问题原因： 依赖rootdir 的 em_debug_create_pd(在fs_initcall(em_debug_init); 前被调用，导致rootdir为空（rootdir 是全局变量，em_debug_init进行初始化）
修改方法：  从log 看，函数调用和初始化挨着很近，但是还是调用在前。所以调整init初始化时间点，将fs_initcall 改为subsys_initcall ,由原来开机4s初始化到3s初始化，其他不变
```


- insmod: ERROR: could not insert module xxxxx.ko: Unknown symbol in module
```shell
<!-- 通过 dmesg 查看 -->
dmesg -w

[  300.333603] livepatch_sample: loading out-of-tree module taints kernel.
[  300.333674] livepatch_sample: module verification failed: signature and/or required key missing - tainting kernel
[  300.333747] livepatch_sample: Unknown symbol __x86_return_thunk (err 0)

<!-- Makefile -->
CONFIG_MODULE_SIG=n

MODULE_INFO(intree, "Y");
```


- loading out-of-tree module taints kernel
```shell
MODULE_INFO(intree, "Y");
```

- Unknown symbol __x86_return_thunk (err 0)
```shell
cat /proc/kallsyms | grep "__x86_return_thunk"
```

- disagrees about version of symbol module_layout
```shell
Makefile里设置kernel源码的路径错误，没有和当前的内核版本一致，导致版本验证不通过，无法安装
```

- error: implicit declaration of function ‘within_module’ [-Werror=implicit-function-declaration]
```shell
1. 
<!-- 在需要调用该函数的文件中声明该函数 -->
extern bool within_module(unsigned long addr, const struct module *mod);

2. 
<!-- 也可在相应.h文件中声明函数 -->
```

- rmmod: ERROR: Module hello is in use
```shell
https://blog.csdn.net/gatieme/article/details/75108154

force_rmmod
```

- 编译 mod 内核版本不对
```shell
<!-- 指定版本 -->
sudo yum install "kernel-devel-uname-r == $(uname -r)"
```

- 'make menuconfig' requires the ncurses libraries.
```shell
<!-- paru -S ncurses5-compat-libs -->


<!-- lvim scripts/kconfig/lxdialog/check-lxdialog.sh -->
--- a/scripts/kconfig/lxdialog/check-lxdialog.sh
+++ b/scripts/kconfig/lxdialog/check-lxdialog.sh
@@ -63,7 +63,7 @@ trap "rm -f $tmp ${tmp%.tmp}.d" 0 1 2 3 15
 check() {
         $cc -x c - -o $tmp 2>/dev/null <<'EOF'
 #include CURSES_LOC
-main() {}
+int main() {}
 EOF
 	if [ $? != 0 ]; then
 	    echo " *** Unable to find the ncurses libraries or the"       1>&2
```


- cc1: error: code model kernel does not support PIC mode
```shell
KBUILD_CFLAGS 尾部加 -fno-pie
```

- include/linux/compiler-gcc.h:106:1: fatal error: linux/compiler-gcc14.h: No such file or directory
```shell
提示缺少compiler-gcc14.h这个文件，是由于内核版本较低和我的gcc版本不匹配造成:

重装一个版本低一点的gcc。

<!-- 或 -->

cp include/linux/compiler-gcc4.h include/linux/compiler-gcc14.h
```
