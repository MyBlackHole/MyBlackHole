# Buildroot

```shell
git://git.buildroot.net/buildroot
```

- build
```shell
make menuconfig
## Target options --->
##     Target Architecture --->
##         (X) x86_64
## Toolchain --->
##     Toolchain type --->
##         (X) Buildroot toolchain
##     C library --->
##         (X) glibc
##     Kernel Headers
##         (X) Linux 5.15.x kernel headers
##     [*] Install glibc utilities
##     [*] Enable C++ support
##     [*] Build cross gdb for the host
##     [*] TUI support
##     [*] Python support
##     [*] Simulator support
## Filesystem images --->
##     [*] ext2/3/4 root filesystem
##         ext2/3/4 variant --->
##             (X) ext4
## Bootloaders --->
##     [*] grub2
##     [ ] i386-pc
##     [ ] i386-efi
##     [*] x86-64-efi
##     (boot linux ext4 fat squash4 part_msdos part_gpt normal efi_gop) builtin modules

export PATH=/usr/bin:/bin

make -j $(nproc) # 开始编译

## 编译完成后，在output/images/目录下可以找到编译好的镜像文件。
```

- make sdk
```shell
<!-- Buildroot 编译时会生成 toolchain，相关内容位于 output/host/ 目录。 -->
<!-- 也可以将toolchain的内容都导出来，把开发中使用的packages一起打包成一个SDK，使用这个SDK就可以编译我们的应用程序，而不需要依赖当前的嵌入式Linux系统，使开发更加方便。-->

make sdk
```

- help
```shell
❯ make help
Cleaning:
  clean                  - delete all files created by build
  distclean              - delete all non-source files (including .config)

Build:
  all                    - make world
  toolchain              - build toolchain
  sdk                    - build relocatable SDK

Configuration:
  menuconfig             - interactive curses-based configurator
  nconfig                - interactive ncurses-based configurator
  xconfig                - interactive Qt-based configurator
  gconfig                - interactive GTK-based configurator
  oldconfig              - resolve any unresolved symbols in .config
  syncconfig             - Same as oldconfig, but quietly, additionally update deps
  olddefconfig           - Same as syncconfig but sets new symbols to their default value
  randconfig             - New config with random answer to all options
  defconfig              - New config with default answer to all options;
                             BR2_DEFCONFIG, if set on the command line, is used as input
  savedefconfig          - Save current config to BR2_DEFCONFIG (minimal config)
  update-defconfig       - Same as savedefconfig
  allyesconfig           - New config where all options are accepted with yes
  allnoconfig            - New config where all options are answered with no
  alldefconfig           - New config where all options are set to default
  randpackageconfig      - New config with random answer to package options
  allyespackageconfig    - New config where pkg options are accepted with yes
  allnopackageconfig     - New config where package options are answered with no

Package-specific:
  <pkg>                  - Build and install <pkg> and all its dependencies
  <pkg>-source           - Only download the source files for <pkg>
  <pkg>-extract          - Extract <pkg> sources
  <pkg>-patch            - Apply patches to <pkg>
  <pkg>-depends          - Build <pkg>'s dependencies
  <pkg>-configure        - Build <pkg> up to the configure step
  <pkg>-build            - Build <pkg> up to the build step
  <pkg>-show-info        - generate info about <pkg>, as a JSON blurb
  <pkg>-show-depends     - List packages on which <pkg> depends
  <pkg>-show-rdepends    - List packages which have <pkg> as a dependency
  <pkg>-show-recursive-depends
                         - Recursively list packages on which <pkg> depends
  <pkg>-show-recursive-rdepends
                         - Recursively list packages which have <pkg> as a dependency
  <pkg>-graph-depends    - Generate a graph of <pkg>'s dependencies
  <pkg>-graph-rdepends   - Generate a graph of <pkg>'s reverse dependencies
  <pkg>-dirclean         - Remove <pkg> build directory
  <pkg>-reconfigure      - Restart the build from the configure step
  <pkg>-rebuild          - Restart the build from the build step
  <pkg>-reinstall        - Restart the build from the install step

busybox:
  busybox-menuconfig     - Run busybox menuconfig
  busybox-xconfig        - Run busybox xconfig
  busybox-gconfig        - Run busybox gconfig
  busybox-update-config  - Save the busybox configuration as a full .config file
                             to package/busybox/busybox.config
                             (or override with BUSYBOX_KCONFIG_FILE)

Documentation:
  manual                 - build manual in all formats
  manual-html            - build manual in HTML
  manual-split-html      - build manual in split HTML
  manual-pdf             - build manual in PDF
  manual-text            - build manual in text
  manual-epub            - build manual in ePub
  graph-build            - generate graphs of the build times
  graph-depends          - generate graph of the dependency tree
  graph-size             - generate stats of the filesystem size
  list-defconfigs        - list all defconfigs (pre-configured minimal systems)

Miscellaneous:
  source                 - download all sources needed for offline-build
  external-deps          - list external packages used
  legal-info             - generate info about license compliance
  show-info              - generate info about packages, as a JSON blurb
  pkg-stats              - generate info about packages as JSON and HTML
  printvars              - dump internal variables selected with VARS=...
  show-vars              - dump all internal variables as a JSON blurb; use VARS=...
                           to limit the list to variables names matching that pattern

  make V=0|1             - 0 => quiet build (default), 1 => verbose build
  make O=dir             - Locate all output files in "dir", including .config

For further details, see README, generate the Buildroot manual, or consult
it on-line at http://buildroot.org/docs.html
```

## error

- Makefile.legacy:9: *** "You have legacy configuration in your .config! Please check your configuration.".  Stop.
```shell
删除.config文件，重新配置
```

- 
You seem to have the current working directory in your
PATH environment variable. This doesn't work.
make: *** [support/dependencies/dependencies.mk:27: dependencies] Error 1
```shell
删除PATH环境变量中的当前工作目录

export PATH=/usr/bin:/bin

<!-- unset LD_LIBRARY_PATH -->
```

- ....stamp_host_installed] Error 1
```shell
make clean world
```
