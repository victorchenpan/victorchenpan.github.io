# <font color="red">Compile uboot</font>
## 一.概述：
 众所周知，如果要跑linux kernel(此时先不讨论挂载文件系统这些工作)，我们得先有一个bootloader,那下面就说下其中的一个embedded bootloader-----uboot;一般的开发流程就是：
 a.先下载uboot源码，在主机上通过交叉编译器得到uboot.bin等映像文件，b.然后下载到flash当中，（如果之前开发板中就已经有了bootloader的话，可以通过tftp把主机上的映像先下载到ram中，再写入到flash或emmc中去），c.修改完环境变量再写入到flash中，然后重启就完成了整个bootloader的移植工作。因此最主要的第一步就是编译出合适的uboot映像文件出来。

---
 ## 二.源码的获取：
- a.从硬件开发商官网中下载符合自己板卡的uboot代码库。
- b.从git://git.denx.de/u-boot.git下载board中含有自己开发板的代码库，虽然后面新版本的大部分都包含以前的旧库，但还是不建议下载最新的。

---
 ## 三.源码的编译：
 - a.从硬件开发商官网下载的代码是经开发商修改好了的，我们可以节省很多时间，因此我们只要查看.../README就可以快速的修改成更适合自己的代码库。
 - b.从git://git.denx.de/u-boot.git下载的代码库，我们要修改的东西就多了点，先在makefile中指定好CORSS_COMPILE,通过make menuconfig 选择好cpu的架构，开发板类型名，uboot命令等选项，保存生成我们的.config文件（如果.../configs下有符合的配置文件的话，可以直接make xxx_deconfig）,然后make all。不过make all通常不会很顺利，会有各种报错产生，一些常规的报错好解决，有时会出现一些我们从没遇到过的错误，通过google也很难找到好方法。比如今天我遇到了一个错误：arm-linux-ld: BFD (GNU Binutils) 2.20.1.20100303 assertion fail /work/toolchain/build/src/binutils-2.20.1/bfd/elf32-arm.c:12195
arm-linux-ld: BFD (GNU Binutils) 2.20.1.20100303 assertion fail /work/toolchain/build/src/binutils-2.20.1/bfd/elf32-arm.c:12195
arm-linux-ld: BFD (GNU Binutils) 2.20.1.20100303 assertion fail /work/toolchain/build/src/binutils-2.20.1/bfd/elf32-arm.c:12195
arm-linux-ld: BFD (GNU Binutils) 2.20.1.20100303 assertion fail /work/toolchain/build/src/binutils-2.20.1/bfd/elf32-arm.c:12195
arm-linux-ld: BFD (GNU Binutils) 2.20.1.20100303 assertion fail /work/toolchain/build/src/binutils-2.20.1/bfd/elf32-arm.c:12195
arm-linux-ld: BFD (GNU Binutils) 2.20.1.20100303 assertion fail /work/toolchain/build/src/binutils-2.20.1/bfd/elf32-arm.c:12195
arm-linux-ld: BFD (GNU Binutils) 2.20.1.20100303 assertion fail /work/toolchain/build/src/binutils-2.20.1/bfd/elf32-arm.c:12195
arm-linux-ld: BFD (GNU Binutils) 2.20.1.20100303 assertion fail /work/toolchain/build/src/binutils-2.20.1/bfd/elf32-arm.c:12429
Segmentation fault
Makefile:1171: recipe for target 'u-boot' failed
make: *** [u-boot] Error 139
大家如果有解决方法望赐教！
回寝室继续找方法！
